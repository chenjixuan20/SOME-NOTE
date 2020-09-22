## Docker

### Dockerfile

在编写Dockerfile后，构建容器镜像时，执行

```shell
docker build -t getting-started .
```

会存在找不到Dockerfile文件的问题。

原因：

​	文本编译器可能会给Dockerfile加上txt拓展名，如：

![image-20200922105028502](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200922105028502.png)

使用下面指令以解决问题：

```shell
docker build -t docker-whale -f ./Dockerfile.txt .
```

getting-started，docker-whale是镜像名。

则创建成功：

![image-20200922105646225](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200922105646225.png)

然后运行下面指令则成功跑起来了

```shell
docker run -dp 3000:3000 docker-whale
```

![image-20200922110132633](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200922110132633.png)

可以看到Docker的Dashboard中有端口为3000的容器产生

![image-20200922110319722](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200922110319722.png)

在浏览器中运行localhost:3000，则访问成功

![image-20200922110044126](/Users/chenjixuan/Library/Application Support/typora-user-images/image-20200922110044126.png)