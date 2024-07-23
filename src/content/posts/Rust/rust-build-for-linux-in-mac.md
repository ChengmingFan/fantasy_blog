---
title: 如何在MacOS上为Ubuntu等linux系统编译可执行文件
published: 2024-07-24
description: rust交叉编译之在MacOS平台为Ubuntu系统编译可执行文件
tags: [Rust, Ubuntu]
category: Rust
draft: false
---

## 在Mac上安装依赖
```shell rustup target add x86_64-unknown-linux-gnu```  
```shell brew tap SergioBenitez/osxct```  
```shell brew install x86_64-unknown-linux-gnu```  

## 配置rust项目
在Rust项目根目录添加一个文件: .cargo/config.toml, 并添加以下内容:
```toml
[target.x86_64-unknown-linux-gnu]
linker = "x86_64-unknown-linux-gnu-gcc"
```

## 执行打包命令
```shell
cargo build --release --target=x86_64-unknown-linux-gnu
```

## 错误异常
最开始少执行了```shell brew tap SergioBenitez/osxct```这个命令, 导致在build时提示错误: 
```text
error: linking with `cc` failed: exit code: 1
note: "cc" "-Wl,--as-needed" "-Wl,-z,noexecstack" "-L" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib" "/Users/cflewis/Documents/Computing/rust/tessel/target/mipsel-unknown-linux-gnu/debug/tessel.0.o" "-o" "/Users/cflewis/Documents/Computing/rust/tessel/target/mipsel-unknown-linux-gnu/debug/tessel" "-Wl,--gc-sections" "-pie" "-nodefaultlibs" "-L" "/Users/cflewis/Documents/Computing/rust/tessel/target/mipsel-unknown-linux-gnu/debug" "-L" "/Users/cflewis/Documents/Computing/rust/tessel/target/mipsel-unknown-linux-gnu/debug/deps" "-L" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib" "-Wl,-Bstatic" "-Wl,-Bdynamic" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/libstd-d16b8f0e.rlib" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/libcollections-d16b8f0e.rlib" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/librustc_unicode-d16b8f0e.rlib" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/librand-d16b8f0e.rlib" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/liballoc-d16b8f0e.rlib" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/liballoc_jemalloc-d16b8f0e.rlib" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/liblibc-d16b8f0e.rlib" "/Users/cflewis/.multirust/toolchains/stable-x86_64-apple-darwin/lib/rustlib/mipsel-unknown-linux-gnu/lib/libcore-d16b8f0e.rlib" "-l" "dl" "-l" "pthread" "-l" "gcc_s" "-l" "pthread" "-l" "c" "-l" "m" "-l" "rt" "-l" "util" "-l" "compiler-rt"
note: clang: warning: argument unused during compilation: '-pie'
ld: unknown option: --as-needed
```
最终在rust仓库中的一个closed的[issue](https://github.com/rust-lang/rust/issues/34282#issuecomment-796182029)中找到解决方法。如果你和我遇到的异常一样, 可以试试上边的方法~