---
title: java中用Thumbnailator对图片各种处理的方法（可做到对原图片压缩仅改变大小）
date: 2017-11-03 14:07:45
tags: [java]
---
[文章来源:java中用Thumbnailator对图片各种处理的方法（可做到对原图片压缩仅改变大小）](http://blog.csdn.net//u011229848//article/details/78434914)

java开发中经常遇到对图片的处理，JDK中也提供了对应的工具类，不过处理起来很麻烦，Thumbnailator是一个优秀的图片处理的开源Java类库,处理效果远比Java API的好，从API提供现有的图像文件和图像对象的类中简化了处理过程，两三行代码就能够从现有图片生成处理后的图片，且允许微调图片的生成方式，同时保持了需要写入的最低限度的代码量。还支持对一个目录的所有图片进行批量处理操作，下边就和大家分享一下java中用Thumbnailator做图片各种处理的方法.

jar自己网上下载一下，maven的话添加依赖即可
<!--more-->
```xml
<!-- https://mvnrepository.com/artifact/net.coobird/thumbnailator -->
<dependency>
    <groupId>net.coobird</groupId>
    <artifactId>thumbnailator</artifactId>
    <version>0.4.8</version>
    
</dependency>
```
 
```java
import java.awt.image.BufferedImage;  
import java.io.File;  
import java.io.FileOutputStream;  
import java.io.IOException;  
import java.io.OutputStream;  
  
import javax.imageio.ImageIO;  
  
import net.coobird.thumbnailator.Thumbnails;  
import net.coobird.thumbnailator.geometry.Positions;  
  
public class ThumbnailatorTest {  
  
    /** 
     *  
     * @param args 
     * @throws IOException 
     */  
    public static void main(String[] args) throws IOException {  
        ThumbnailatorTest thumbnailatorTest = new ThumbnailatorTest();  
        thumbnailatorTest.test1();  
        thumbnailatorTest.test2();  
        thumbnailatorTest.test3();  
        thumbnailatorTest.test4();  
        thumbnailatorTest.test5();  
        thumbnailatorTest.test6();  
        thumbnailatorTest.test7();  
        thumbnailatorTest.test8();  
        thumbnailatorTest.test9();  
    }  
  
    /** 
     * 指定大小进行缩放 
     *  
     * @throws IOException 
     */  
    private void test1() throws IOException {  
        /* 
         * size(width,height) 若图片横比200小，高比300小，不变 
         * 若图片横比200小，高比300大，高缩小到300，图片比例不变 若图片横比200大，高比300小，横缩小到200，图片比例不变 
         * 若图片横比200大，高比300大，图片按比例缩小，横为200或高为300 
         */  
        Thumbnails.of("images/test.jpg").size(200, 300).toFile(  
                "C:/image_200x300.jpg");  
        Thumbnails.of("images/test.jpg").size(2560, 2048).toFile(  
                "C:/image_2560x2048.jpg");  
    }  
  
    /** 
     * 按照比例进行缩放 
     *  
     * @throws IOException 
     */  
    private void test2() throws IOException {  
        /** 
         * scale(比例) 
         */  
        Thumbnails.of("images/test.jpg").scale(0.25f)  
                .toFile("C:/image_25%.jpg");  
        Thumbnails.of("images/test.jpg").scale(1.10f).toFile(  
                "C:/image_110%.jpg");  
    }  
  
    /** 
     * 不按照比例，指定大小进行缩放 
     *  
     * @throws IOException 
     */  
    private void test3() throws IOException {  
        /** 
         * keepAspectRatio(false) 默认是按照比例缩放的 
         */  
        Thumbnails.of("images/test.jpg").size(120, 120).keepAspectRatio(false)  
                .toFile("C:/image_120x120.jpg");  
    }  
  
    /** 
     * 旋转 
     *  
     * @throws IOException 
     */  
    private void test4() throws IOException {  
        /** 
         * rotate(角度),正数：顺时针 负数：逆时针 
         */  
        Thumbnails.of("images/test.jpg").size(1280, 1024).rotate(90).toFile(  
                "C:/image+90.jpg");  
        Thumbnails.of("images/test.jpg").size(1280, 1024).rotate(-90).toFile(  
                "C:/iamge-90.jpg");  
    }  
  
    /** 
     * 水印 
     *  
     * @throws IOException 
     */  
    private void test5() throws IOException {  
        /** 
         * watermark(位置，水印图，透明度) 
         */  
        Thumbnails.of("images/test.jpg").size(1280, 1024).watermark(  
                Positions.BOTTOM_RIGHT,  
                ImageIO.read(new File("images/watermark.png")), 0.5f)  
                .outputQuality(0.8f).toFile(  
                        "C:/image_watermark_bottom_right.jpg");  
        Thumbnails.of("images/test.jpg").size(1280, 1024).watermark(  
                Positions.CENTER,  
                ImageIO.read(new File("images/watermark.png")), 0.5f)  
                .outputQuality(0.8f).toFile("C:/image_watermark_center.jpg");  
    }  
  
    /** 
     * 裁剪 
     *  
     * @throws IOException 
     */  
    private void test6() throws IOException {  
        /** 
         * 图片中心400*400的区域 
         */  
        Thumbnails.of("images/test.jpg").sourceRegion(Positions.CENTER, 400,  
                400).size(200, 200).keepAspectRatio(false).toFile(  
                "C:/image_region_center.jpg");  
        /** 
         * 图片右下400*400的区域 
         */  
        Thumbnails.of("images/test.jpg").sourceRegion(Positions.BOTTOM_RIGHT,  
                400, 400).size(200, 200).keepAspectRatio(false).toFile(  
                "C:/image_region_bootom_right.jpg");  
        /** 
         * 指定坐标 
         */  
        Thumbnails.of("images/test.jpg").sourceRegion(600, 500, 400, 400).size(  
                200, 200).keepAspectRatio(false).toFile(  
                "C:/image_region_coord.jpg");  
    }  
  
    /** 
     * 转化图像格式 
     *  
     * @throws IOException 
     */  
    private void test7() throws IOException {  
        /** 
         * outputFormat(图像格式) 
         */  
        Thumbnails.of("images/test.jpg").size(1280, 1024).outputFormat("png")  
                .toFile("C:/image_1280x1024.png");  
        Thumbnails.of("images/test.jpg").size(1280, 1024).outputFormat("gif")  
                .toFile("C:/image_1280x1024.gif");  
    }  
  
    /** 
     * 输出到OutputStream 
     *  
     * @throws IOException 
     */  
    private void test8() throws IOException {  
        /** 
         * toOutputStream(流对象) 
         */  
        OutputStream os = new FileOutputStream(  
                "C:/image_1280x1024_OutputStream.png");  
        Thumbnails.of("images/test.jpg").size(1280, 1024).toOutputStream(os);  
    }  
  
    /** 
     * 输出到BufferedImage 
     *  
     * @throws IOException 
     */  
    private void test9() throws IOException {  
        /** 
         * asBufferedImage() 返回BufferedImage 
         */  
        BufferedImage thumbnail = Thumbnails.of("images/test.jpg").size(1280,  
                1024).asBufferedImage();  
        ImageIO.write(thumbnail, "jpg", new File(  
                "C:/image_1280x1024_BufferedImage.jpg"));  
    }  
}
```