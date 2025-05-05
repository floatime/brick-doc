# 文件存储工具组件

>前言
>
>文件存储工具组件可以在 SpringBoot 中通过简单的方式将文件存储到 本地、FTP、SFTP、WebDAV、阿里云 OSS等平台
>
>此文档是对 X Spring File Storage 快速上手说明
>
>x spring file storage官网: https://spring-file-storage.xuyanwu.cn/#/

## 引入依赖
> 引入对应平台的依赖，用不到平台不用引入
>
> 这里我们的项目用到的时aliyun_oss,那就只引入阿里云的依赖就好

```xml
<!--必须引入-->
<dependency>
    <groupId>cn.xuyanwu</groupId>
    <artifactId>x-spring-file-storage</artifactId>
    <version>0.7.0.f</version>
</dependency>

<!--aliyun-oss依赖-->
<dependency>
    <groupId>com.aliyun.oss</groupId>
    <artifactId>aliyun-sdk-oss</artifactId>
    <version>3.15.1</version>
</dependency>
```

## 文件服务配置

```yml
#  配置:结合实际需求(参考最全的配置文件 选择项目中所用到的)
xfile:
  config:
  	# #默认使用的存储平台
    defaultPlatform: oss
    aliyun-oss:
    	# 存储平台标识
      - platform: oss       
      	# 启用存储
        enable-storage: true
        access-key: 阿里云oss的access-key
        secret-key: 阿里云oss的secret-key
        end-point: 阿里云oss的end-point
        bucket-name: 阿里云oss的bucket-name
        # 访问域名，注意“/”结尾
        domain: https://域名.com/
```

## 文件上传工具使用

```java
package cn.yiqu.app.inspector.impl;

import cn.hutool.core.util.IdUtil;
import cn.ifloat.brick.basic.common.toolkit.date.DateUtil;
import cn.ifloat.brick.logger.core.LogKit;
import cn.xuyanwu.spring.file.storage.FileInfo;
import cn.xuyanwu.spring.file.storage.FileStorageService;
import cn.yiqu.app.inspector.service.OSSService;
import org.springframework.stereotype.Service;
import org.springframework.web.multipart.MultipartFile;

import javax.annotation.Resource;
import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.util.Date;

@Service
public class OSSImpl implements OSSService {

    @Resource
    private FileStorageService fileStorageService;

    /**
     * 获取文件名所对应的ContentType
     */
    public static String getContentType(String FilenameExtension) {
        if (FilenameExtension.equalsIgnoreCase(".bmp")) {
            return "image/bmp";
        }
        if (FilenameExtension.equalsIgnoreCase(".gif")) {
            return "image/gif";
        }
        if (FilenameExtension.equalsIgnoreCase(".jpeg") ||
                FilenameExtension.equalsIgnoreCase(".jpg") ||
                FilenameExtension.equalsIgnoreCase(".png")) {
            return "image/jpg";
        }
        if (FilenameExtension.equalsIgnoreCase(".html")) {
            return "text/html";
        }
        if (FilenameExtension.equalsIgnoreCase(".txt")) {
            return "text/plain";
        }
        if (FilenameExtension.equalsIgnoreCase(".vsd")) {
            return "application/vnd.visio";
        }
        if (FilenameExtension.equalsIgnoreCase(".pptx") ||
                FilenameExtension.equalsIgnoreCase(".ppt")) {
            return "application/vnd.ms-powerpoint";
        }
        if (FilenameExtension.equalsIgnoreCase(".docx") ||
                FilenameExtension.equalsIgnoreCase(".doc")) {
            return "application/msword";
        }
        if (FilenameExtension.equalsIgnoreCase(".xml")) {
            return "text/xml";
        }
        if (FilenameExtension.equalsIgnoreCase(".pdf")) {
            return "application/pdf";
        }
        return "image/jpg";
    }

    /**
     * 存储MultipartFile文件到阿里云OSS  返回文件url路径
     *
     * @param bytes
     * @return url
     */
    @Override
    public String uploadFile(MultipartFile file, String directory) {
        //构建日期路径：avatar/2019/02/26/文件名
        //directory 表示此文件位于阿里云OSS中bucket下的目录结构
        String filePath = directory + DateUtil.datetimeToStr(new Date(), "yyyy/MM/dd/");
        String url = null;
        String fileName = file.getOriginalFilename();
        try {
            assert fileName != null;
            FileInfo info = fileStorageService
                    .of(file.getInputStream())
                    //设置原始文件名
                    .setOriginalFilename(file.getOriginalFilename())
                    //设置文件ContentType
                    .setContentType(getContentType(fileName.substring(fileName.lastIndexOf("."))))
                    //设置文件存储路径
                    .setPath(filePath)
                    .upload();
            url = info.getUrl();
        } catch (IOException e) {
            throw new RuntimeException(e);
        }
        LogKit.system().info("url:" + url);
        return url;
    }

    /**
     * 存储字节数据到阿里云OSS  返回文件url路径
     *
     * @param bytes
     * @return url
     */
    @Override
    public String uploadExcel(byte[] bytes) {
        InputStream inputStream = new ByteArrayInputStream(bytes);
        //构建日期路径：avatar/2019/02/26/文件名
        //directory 表示此文件位于阿里云OSS中bucket下的目录结构
        String filePath = "excel/" + DateUtil.datetimeToStr(new Date(), "yyyy/MM/dd/");
        String fileName = IdUtil.randomUUID() + ".xlsx";
        FileInfo info = fileStorageService
                .of(inputStream)
                .setOriginalFilename(fileName)
                .setContentType("xls/xlsx")
                .setPath(filePath)
                .upload();
        String url = info.getUrl();
        LogKit.system().info("url:" + url);
        return url;
    }
}
```

