# gsoc
A summary of my activities during gsoc 2021.

Done:  
- Enable [llvm-plugins](https://github.com/rust-lang/rust/pull/86267) on nightly (Enzyme _is_ an LLVM plugin, so this probably makes sense)  
- Add [build flags](https://github.com/rust-lang/rust/pull/87297) to build LLVM with it's plugin interface and clang  
- Create [enzyme\_build](https://github.com/ZuseZ4/enzyme_build) which will build rustc/clang/enzyme/llvm with all the required flag's in the right configuration. 
- Create [https://rust-ml.github.io/oxide-enzyme](https://rust-ml.github.io/oxide-enzyme) which should help users 
        with setting up Enzyme and picking the right functionality for their use-cases.  
- Update [oxide-enzyme](https://github.com/rust-ml/oxide-enzyme).  
    - Strip broken build support.  
    - Replace external deps like objcopy with LLVM functions.  
    - Reduce linux specific elements like Paths.  
    - Update to Rust 1.54.  
    - Update to the latest CApi from Enzyme.  
    - Add support to generate multiple gradient functions.  
    - Enable gradient optimizations.  
    - Add extra handling for gradient functions returning a struct {T}.  
        Rust expects them to be simplified to T, so we mimic this behavior by creating a wrapper.  
 

Unfinished:  
- Create two proc-macros. #[Enzyme] to mark primary functions and enzyme!(..) to call the gradient functions with the desired settings. They are technically not required. However they should provide multiple convenience and security improvements like:  
  - Simply marking primary functions based on which gradients should be generated.  
  - Pointing out how Enzyme will use which input variable.  
  - Assuring that input variables will be dropped safely if accessing them after an enzyme invocation is UB.  
  - Adding a simple way to configure how Enzyme will handle input variables.  
  - Hide unsafe sections  

Reason: My understanding of Enzyme and the Rust build process improved during the gsoc period. Repeated iteration were helpful for the learning purpose but ended in a broken state. A new attempt will be done and added to oxide-enzyme as a subcrate.  
Note: Some of these goals could probably be achieved cleaner by rustc modifications / integrations.  
Rust(c) however intents to not only support LLVM, but also GCC and other compilers as backend. This blocks some solutions, as they might not make sense when using a non-LLVM backend.  
