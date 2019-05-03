# ASM

## 1. 前言

    前几天,在Q群里有个大佬,展示了下 Android 做无痕埋点,觉得挺厉害的
    问了下使用的是 AspectJ, 网上搜了下资料 ASM 比 AspectJ 更灵活,更轻量
    刚好趁着五一假期系统的学习下

## 2. 介绍

    ASM 是一款轻量级的Java字节码操作仓库

## 3. 前期准备

### 3.1 简单的asm 方面的知识

    ASM 主要有几个类需要了解 而且需要对 Java字节码 比较熟悉

    ClassReader
        字节码的读取与分析引擎。它采用类似SAX的事件读取机制，每当有事件发生时，调用注册的ClassVisitor、AnnotationVisitor、FieldVisitor、MethodVisitor做相应的处理。

    ClassVisitor
        定义在读取Class字节码时会触发的事件，如类头解析完成、注解解析、字段解析、方法解析等

    AnnotationVisitor
        定义在解析注解时会触发的事件，如解析到一个基本值类型的注解、enum值类型的注解、Array值类型的注解、注解值类型的注解等

    FieldVisitor
        定义在解析字段时触发的事件，如解析到字段上的注解、解析到字段相关的属性等
        
    MethodVisitor
        定义在解析方法时触发的事件，如方法上的注解、属性、代码等。

    ClassWriter
        它实现了ClassVisitor接口，用于拼接字节码。


### 3.2 开发工具准备

    idea / Android studio 
    ASM Bytecode Viewer(对 Java字节码 不熟悉的话必备)

## 4 实战

### 4.1 要实现的效果

``` java
class Hello {
    public static void main(String[] args) {
        show();
    }

    public static void show(){
        System.out.println("Hello World");
    }
}
```

    上图为一个 Hello 类,要对 show() 方法的耗时进行计算并打印

### 4.2 编写 ASM 逻辑

#### 4.2.1 编写 ClassVisitor

    解析类的监听器,解析Class字节码时会触发内部的方法

``` java
public class TestClassVisitor extends ClassVisitor {
    public TestClassVisitor(final ClassVisitor cv) {
        super(Opcodes.ASM5, cv);
    }

    @Override
    public void visit(int version, int access, String name, String signature, String superName, String[] interfaces) {
        if (cv != null) {
            cv.visit(version, access, name, signature, superName, interfaces);
        }
    }

    @Override
    public MethodVisitor visitMethod(int access, String name, String desc, String signature, String[] exceptions) {
        //如果methodName是show，则返回我们自定义的TestMethodVisitor
        if ("show".equals(name)) {
            MethodVisitor mv = cv.visitMethod(access, name, desc, signature, exceptions);
            return new TestMethodVisitor(mv);
        }
        if (cv != null) {
            return cv.visitMethod(access, name, desc, signature, exceptions);
        }
        return null;
    }
}
```

#### 4.2.2 编写 MethodVisitor

    解析方法的监听器,解析Method时会触发内部的方法
    编写前若对 Java字节码 不熟悉的话 
        建议安装 ASM Bytecode Viewer 插件!!!
        建议安装 ASM Bytecode Viewer 插件!!!
        建议安装 ASM Bytecode Viewer 插件!!!

    先新建一个类 编写要注入的代码,然后用插件查看

![image.png](https://user-gold-cdn.xitu.io/2019/5/1/16a72ffe49deac91?w=1240&h=658&f=png&s=372454)

``` java
public class TestMethodVisitor extends MethodVisitor implements Opcodes {
    public TestMethodVisitor(MethodVisitor mv) {
        super(Opcodes.ASM5, mv);
    }

    @Override
    public void visitCode() {
        //方法体内开始时调用
        mv.visitMethodInsn(INVOKESTATIC, "java/lang/System", "currentTimeMillis", "()J", false);
        mv.visitVarInsn(LSTORE, 0);
        super.visitCode();
    }
    @Override
    public void visitInsn(int opcode) {
        //每执行一个指令都会调用
        if (opcode == Opcodes.RETURN) {
            mv.visitMethodInsn(INVOKESTATIC, "java/lang/System", "currentTimeMillis", "()J", false);
            mv.visitVarInsn(LLOAD, 0);
            mv.visitInsn(LSUB);
            mv.visitVarInsn(LSTORE, 2);
            Label l3 = new Label();
            mv.visitLabel(l3);
            mv.visitLineNumber(11, l3);
            mv.visitFieldInsn(GETSTATIC, "java/lang/System", "out", "Ljava/io/PrintStream;");
            mv.visitTypeInsn(NEW, "java/lang/StringBuilder");
            mv.visitInsn(DUP);
            mv.visitMethodInsn(INVOKESPECIAL, "java/lang/StringBuilder", "<init>", "()V", false);
            mv.visitLdcInsn("== method cost time = ");
            mv.visitMethodInsn(INVOKEVIRTUAL, "java/lang/StringBuilder", "append", "(Ljava/lang/String;)Ljava/lang/StringBuilder;", false);
            mv.visitVarInsn(LLOAD, 2);
            mv.visitMethodInsn(INVOKEVIRTUAL, "java/lang/StringBuilder", "append", "(J)Ljava/lang/StringBuilder;", false);
            mv.visitLdcInsn(" ==");
            mv.visitMethodInsn(INVOKEVIRTUAL, "java/lang/StringBuilder", "append", "(Ljava/lang/String;)Ljava/lang/StringBuilder;", false);
            mv.visitMethodInsn(INVOKEVIRTUAL, "java/lang/StringBuilder", "toString", "()Ljava/lang/String;", false);
            mv.visitMethodInsn(INVOKEVIRTUAL, "java/io/PrintStream", "println", "(Ljava/lang/String;)V", false);

        }
        super.visitInsn(opcode);
    }
}
```

### 4.3 测试效果

编写测试类 运行

``` java
public class Demo {
    public static void main(String[] args) throws IOException {
        ClassReader cr = new ClassReader(Hello.class.getName());
        ClassWriter cw = new ClassWriter(cr, ClassWriter.COMPUTE_MAXS);
        ClassVisitor cv = new TestClassVisitor(cw);
        cr.accept(cv, Opcodes.ASM5);
        // 获取生成的class文件对应的二进制流
        byte[] code = cw.toByteArray();
        //将二进制流写到out/下
        FileOutputStream fos = new FileOutputStream("out/Hello.class");
        fos.write(code);
        fos.close();
    }
}
```
 
原 Hello 类生成的 .class 文件,以及输出效果

![image.png](https://upload-images.jianshu.io/upload_images/61189-aa71d43cc5e6734e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-3d9e01aa35219705.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

字节码修改后的 Hello 类的 .class 文件以及输出效果

![image.png](https://upload-images.jianshu.io/upload_images/61189-8779d97d2b452cde.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![image.png](https://upload-images.jianshu.io/upload_images/61189-a7d73d30cf8c8af9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 用途

    可以用于无痕埋点,打印日志,以及性能监控等

## TIPS:

[Github地址](https://github.com/CodeLiuPu/HelloAsm)