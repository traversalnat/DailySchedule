# Daliy LOG

## 2022-07-04
1. 学习文档第一章(应用程序和基本执行环境) 实现 print 输出
2. 学习rustlings 1-43题 包括前两个 quiz

## 2022-07-05
1. 完成 rustlings exercises

## 2022-07-06
1. 试着做了 33 个 Rust quiz，理解了部分quiz 的意思，关于宏的部分仍然不是很理解
    1. rust 中没有 c/c++ 中的 a++ ++a 之类的语法
    2. rust 函数式 map 遵循惰性求值原则，取值时才执行 map
    3. 同时存在 f(&self) 和 f(&mut self) 时，Auto-ref 遵循 &self 优先原则
    4. (x,) 中逗号表示这是一个特殊的单元素元组, (x, y) 与 (x, y,) 没有区别
    5. `let (.., x, y) = (0, 1, ..);` .. 有两种用法，一种表示 RangeFull，一种表示匹配剩余所有值
    6. `let _ = s` 没有移动 s 所有权
    7. match 中 | 的存在会促使 match 尝试 | 左右两边的所有可能
        ```rust
        fn check(x: i32) -> bool {
            print!("{}", x);
            false
        }

        fn main() {
            match (1, 2) {
                (x, _) | (_, x) if check(x) => {
                    print!("3")
                }
                _ => print!("4"),
            }
        }
        ```
    8.  对于 impl, 要记得 Self 的类型
        ```rust
        // Self 类型为 u32, &self 类型为 &u32
        impl Trait for u32 {
            fn f(&self) {
                print!("1");
            }
        }

        // Self 类型为 &i32, &self 类型为 &&i32
        impl<'a> Trait for &'a i32 {
            fn f(&self) {
                print!("2");
            }
        }
        ```
    9. match 中使用 enum 的值时注意使用全称或者使用 use enum::{} 导入
        ```rust
        #[repr(u8)]
        enum Enum {
            First,
            Second,
        }

        impl Enum {
            fn p(self) {
                // 这里 First 和 Second 实际上和 _ 等效, 注意使用 use Enum::* 导入后使用
                match self {
                    First => print!("1"),
                    Second => print!("2"),
                }
            }
        }
        ```
    10. break 和 return 的相同和不同
        1. 相同：都可以返回一个值
        2. 不同：return 总是会将后面的表达式作为一个值返回，break 不一定
            ```rust
            // return 会返回 { print!("2") } 表达式的结果 ()
            fn return2() {
                if return { print!("2") } {
                }
            }
            fn break2() {
                loop {
                    if break { print!("2") } {
                    }
                }
            }
            ```

            break2 等效于
            ```rust
            fn break2() {
                loop {
                    if break { 
                        print!("2") 
                    } 
                    {}
                }
            }
            ```
## 2022-07-07
尝试从头开始实现第一章, 观察更多细节，代码在 [**第一章代码**](annals/os1.md)

## 2022-07-11
尝试第二章部分代码

## 2022-07-22
通过阅读lab0 和 lab1 的代码，理解 risc-v 下模式切换、上下文切换的要点，完成 lab1 ，理解当前任务管理的设计。


## 2022-07-26
经过几天的努力，完成了将前三个实验, 由于地址空间、进程等概念已经学习过，进度还可以，接下来是一直没有理解的文件系统, 希望能够顺利

## 2022-07-31
    完成了文件系统和并发死锁检测两个实验。
    此次实验让我对文件系统有了一个全面的认识， 此次实现的文件系统底层是以 DiskInode 结构实现树型结构来记录文件信息（目录可以看成特殊的文件，每一个目录项记录一个文件名及其对应的 Inode ）, 文件与文件之间构成一颗文件树，根节点是根目录所在节点, 根目录对应的 DiskInode 通常在第一个索引节点区域，通过超级块能够简单的找到。
    并发死锁主要是实现描述的算法，算法实现不难，主要时间花在了与 rust 编译器斗争，由于rust 特有的引用（实际上是指针的包装）、借用、默认只读等等特性，在写函数式调用链时尤其需要注意得到的元素是否是引用，什么时候解引用等。

## 2022-08-02
    使用 cargo check 检查代码是否能够通过编译，（为什么在最后才知道，白白浪费了许多时间）
