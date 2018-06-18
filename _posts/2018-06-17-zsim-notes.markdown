---
layout: single
title:  "Installing ZSim on Ubuntu 16.04 with Pin 2.14"
date:   2018-06-17 19:40:00 -0500
categories: tool
---

- Clone the zsim version from Stanford MAST 

    `git clone https://github.com/stanford-mast/zsim.git` 

- Install libconfig 
    
    Download the source from [here][libconfig]. Install it locally and set $LIBCONFIGPATH to where it is built.

- Install libhdf5 and scons

    `sudo apt-get install libhdf5-dev libelf-dev scons`

- Modify `src/pin_cmd.cpp` to let Pin 2.14 runs on this Linux version. See previous [post]({% post_url 2018-06-17-pin-notes %}).
   
```
--- a/src/pin_cmd.cpp
+++ b/src/pin_cmd.cpp
@@ -71,9 +71,10 @@ PinCmd::PinCmd(Config* conf, const char* configFile, const char* outputDir, uint
     if (PIN_PRODUCT_VERSION_MAJOR <= 2 && LINUX_VERSION_CODE >= KERNEL_VERSION(4,0,0)
             && std::find(args.begin(), args.end(), "-injection") == args.end()) {
         // FIXME(mgao): hack to bypass kernel version check in Pin 2.x.
-        // Parent injection.
+        // Child injection.
         args.push_back("-injection");
-        args.push_back("parent");
+        args.push_back("child");
+        args.push_back("-ifeellucky");

```
 
- Modify `src/zsim.cpp` to [ignore TSX instructions][tsx-issue]

```
--- a/src/zsim.cpp
+++ b/src/zsim.cpp
@@ -564,7 +564,7 @@ VOID Instruction(INS ins) {
         }
 
         // Instrument only conditional branches
-        if (INS_Category(ins) == XED_CATEGORY_COND_BR) {
+        if (INS_Category(ins) == XED_CATEGORY_COND_BR && !INS_IsXend(ins)) {
             INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR) IndirectRecordBranch, IARG_FAST_ANALYSIS_CALL, IARG_THREAD_ID,
                     IARG_INST_PTR, IARG_BRANCH_TAKEN, IARG_BRANCH_TARGET_ADDR, IARG_FALLTHROUGH_ADDR, IARG_END);
         }

```

- Test run

    `export PINPATH=<pin_install_path>`

    `$ZSIM_PATH/build/opt/zsim tests/simple.cfg`


[libconfig]: https://hyperrealm.github.io/libconfig/
[tsx-issue]: https://github.com/s5z/zsim/issues/154
