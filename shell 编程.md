## 用 let 与不用 let 有什么区别?

用 let 后边可以有计算表达式, 不用的话直接赋值为字符串

## 循环怎么写?

```bash
for a in {1..10}
do
        mkdir /datas/aaa$a
        cd /datas/aaa$a
        for b in {1..10}
        do
                mkdir bbb$b
        done
done
```