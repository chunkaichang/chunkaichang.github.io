---
layout: single
title:  "Installing ZSim on Ubuntu 16.04 with Pin 2.14"
date:   2018-06-17 19:40:00 -0500
categories: tool
---

**Update 11/29/2019:** Tested with Ubuntu 18.04 on WSL2.

- Clone the zsim version from Stanford MAST 

    `git clone https://github.com/stanford-mast/zsim.git` 

- Install libconfig 
    
    `sudo apt-get install libconfig-dev libconfig++-dev`

    OR

    Download the source from [here][libconfig]. Install it locally and set $LIBCONFIGPATH to where it is built.

- Install libhdf5 and scons

    `sudo apt-get install libhdf5-dev libelf-dev scons`


- Suppress some warnings
```
--- a/SConstruct
+++ b/SConstruct
@@ -79,7 +79,7 @@ def buildSim(cppFlags, dir, type, pgo=None):
     ##env["CPPFLAGS"] += " -DDEBUG=1"

     # Be a Warning Nazi? (recommended)
-    env["CPPFLAGS"] += " -Werror "
+    ##env["CPPFLAGS"] += " -Werror "
```   

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

## Additional patches for gcc 7.4
```
--- a/src/zsim.cpp
+++ b/src/zsim.cpp
@@ -28,7 +28,8 @@


 #include "zsim.h"
 #include <algorithm>
-#include <bits/signum.h>
+//#include <bits/signum.h>
+#include <signal.h>
@@ -575,7 +576,7 @@ VOID Instruction(INS ins) {
      * is never emitted by any x86 compiler, as they use other (recommended) nop
      * instructions or sequences.
      */
-    if (INS_IsXchg(ins) && INS_OperandReg(ins, 0) == REG_RCX && INS_OperandReg(ins, 1) == REG_RCX) {
+    if (INS_IsXchg(ins) && INS_OperandReg(ins, 0) ==  LEVEL_BASE::REG::REG_RCX && INS_OperandReg(ins, 1) == LEVEL_BASE::REG::REG_RCX) {           //info("Instrumenting magic op");
         INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR) HandleMagicOp, IARG_THREAD_ID, IARG_REG_VALUE, REG_ECX, IARG_END);
     }
@@ -784,11 +785,11 @@ VOID VdsoInstrument(INS ins) {
     if (unlikely(insAddr >= vdsoStart && insAddr < vdsoEnd)) {
         if (vdsoEntryMap.find(insAddr) != vdsoEntryMap.end()) {
             VdsoFunc func = vdsoEntryMap[insAddr];
-            INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR) VdsoEntryPoint, IARG_THREAD_ID, IARG_UINT32, (uint32_t)func, IARG_REG_VALUE, REG_RDI, IARG_REG_VALUE, REG_RSI, IARG_END);
+            INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR) VdsoEntryPoint, IARG_THREAD_ID, IARG_UINT32, (uint32_t)func, IARG_REG_VALUE, LEVEL_BASE::REG::REG_RDI, IARG_REG_VALUE, LEVEL_BASE::REG::REG_RSI, IARG_END);
         } else if (INS_IsCall(ins)) {
             INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR) VdsoCallPoint, IARG_THREAD_ID, IARG_END);
         } else if (INS_IsRet(ins)) {
-            INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR) VdsoRetPoint, IARG_THREAD_ID, IARG_REG_REFERENCE, REG_RAX /* return val */, IARG_END);
+            INS_InsertCall(ins, IPOINT_BEFORE, (AFUNPTR) VdsoRetPoint, IARG_THREAD_ID, IARG_REG_REFERENCE, LEVEL_BASE::REG::REG_RAX /* return val */, IARG_END);
         }
     }
```

- Build

    `scons -j4`

- Test run

    `export PINPATH=<pin_install_path>`

    `$ZSIM_PATH/build/opt/zsim tests/simple.cfg`


[libconfig]: https://hyperrealm.github.io/libconfig/
[tsx-issue]: https://github.com/s5z/zsim/issues/154
