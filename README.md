# gsoc
A summary of my activities during gsoc 2021.

### Done:  
- Enable [llvm-plugins](https://github.com/rust-lang/rust/pull/86267) on nightly (Enzyme _is_ an LLVM plugin, so this probably makes sense)  
- Add [build flags](https://github.com/rust-lang/rust/pull/87297) to build LLVM with it's plugin interface and clang  
- Create [enzyme\_build](https://github.com/ZuseZ4/enzyme_build/commit/ee84fd20cf47a74581fc8a26ca1713539d529dba) which will build rustc/clang/enzyme/llvm with all the required flag's in the right configuration. 
- Create [https://rust-ml.github.io/oxide-enzyme](https://rust-ml.github.io/oxide-enzyme) which should help users 
        with setting up Enzyme and picking the right functionality for their use-cases.  
- Update [oxide-enzyme](https://github.com/rust-ml/oxide-enzyme).  
    - Strip broken build support.  
    - Replace external deps like objcopy with LLVM functions.  
    - Reduce linux specific elements like Paths.  
    - Update to Rust 1.54.  
    - Update to the latest CApi from Enzyme.  
    - Enable gradient optimizations.  
    - Add support to generate multiple gradient functions.  
    - Add extra handling for gradient functions returning a struct {T}.  
        Rust expects them to be simplified to T, so we mimic this behavior by creating a wrapper.  
 

### Unfinished:  
- Create two proc-macros. #[Enzyme] to mark primary functions and enzyme!(..) to call the gradient functions with the desired settings. They are technically not required. However they should provide multiple convenience and security improvements like:  
  - Simply marking primary functions based on which gradients should be generated.  
  - Pointing out how Enzyme will use which input variable.  
  - Assuring that input variables will be dropped safely if accessing them after an enzyme invocation is UB.  
  - Adding a simple way to configure how Enzyme will handle input variables.  
  - Hide unsafe sections  

Reason: My understanding of Enzyme and the Rust build process improved during the gsoc period. Repeated iteration were helpful for the learning purpose but ended in a broken state. A new attempt will be done and added to oxide-enzyme as a subcrate.  
Note: Some of these goals could probably be achieved cleaner by rustc modifications / integrations.  
Rust(c) however intents to not only support LLVM, but also GCC and other compilers as backend. This blocks some solutions, as they might not make sense when using a non-LLVM backend.  


### Next steps:
After landing https://github.com/wsmoses/Enzyme/pull/307 we should be able to add more complex examples.
In combination with new helper-macros we should also point out how users can efficiently use oxide-enzyme on larger crates.
Porting [this](https://github.com/tiberiusferreira/oxide-enzyme/blob/master/src/main.rs) example of a neural network might be a good start.

### Lessons Learned:
While I had OS and compiler lectures in University, actually working with llvm, rustc and enzyme gave me a much better understanding of how things 
work under the hood. I spent a significant amount on debugging and therefore learned how to use tools like nm and ldd 
to figure out where the automated building process fails. 
In combination with an HPC lecture and a performance [issue](https://github.com/rust-lang/rust/issues/85354) I encountered simultaneously 
I used the last months to learn more about the optimizations which a compiler will apply. This will surely help me for the next time
where I have to optimize some code for performance.


### Summary:
Getting Rust and especially Enzyme into the right shape for HPC applications is an ongoing process. 
After [two](https://github.com/tiberiusferreira/oxide-enzyme) [earlier](https://github.com/bytesnake/oxide-enzyme) attempts I feel like gsoc gave this iteration enough momentum to finally make oxide-enzyme a usefull tool for many HPC areas.
It will be interesting to observe which new or existing applications will adopt Enzyme. 
For the next big milestone here we probably want to analyze the growing amount of possibilities to write gpgpu kernels in Rust.
While some of these use their own codegen backend, others are compatible to LLVM and therefore Enzyme. 
This could be used to not only write GPGPU Kernels in Rust, but also generate highly efficient gradient functions for them.
