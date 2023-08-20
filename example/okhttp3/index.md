# okhttp3

## 配置
```xml
<dependency>
    <groupId>com.squareup.okhttp3</groupId>
    <artifactId>okhttp</artifactId>
    <version>4.11.0</version>
</dependency>
```

## 发送文件

```java
@RequestMapping("/send")
    public String send(@RequestParam("file") MultipartFile file, String str) throws IOException {
        RequestBody requestBody = new MultipartBody.Builder()
                .setType(MultipartBody.FORM)
                .addFormDataPart("random", str)
                .addFormDataPart("upload_type", "UPLOAD_BY_FILE")
                .addFormDataPart("video_signature", "md5_value")
                .addFormDataPart("file", file.getOriginalFilename(), RequestBody.create(MediaType.parse("application/octet-stream"), file.getBytes()))
                //.addFormDataPart("video_file", "video_file.mp4", createStreamRequestBody(file.))
                .addFormDataPart("filename", "custom_filename")
                .addFormDataPart("labels", "label1,label2")
                .addFormDataPart("video_url", "http://xxx.xxx")
                .addFormDataPart("is_aigc", "true")
                .build();

        Request request = new Request.Builder()
                .url("http://localhost:8080/revice")
                .post(requestBody)
                .build();
        try (Response response = client.newCall(request).execute()) {
            return response.body().string();
        }
    }
```

## 发送嵌套参数

```java
@RequestMapping("/send/image")
    public String sendImage(@RequestParam("file") MultipartFile file, String str) throws IOException {
        RequestBody requestBody = new MultipartBody.Builder()
                .setType(MultipartBody.FORM)
                .addFormDataPart("random", str)
                .addFormDataPart("upload_type", "UPLOAD_BY_FILE")
                .addFormDataPart("video_signature", "md5_value")
                .addFormDataPart("image_file[data]", file.getOriginalFilename(), RequestBody.create(MediaType.parse("application/octet-stream"), file.getBytes()))
                //.addFormDataPart("video_file", "video_file.mp4", createStreamRequestBody(file.))
                .addFormDataPart("image_file[file_name]", "custom_filename")
                .addFormDataPart("labels", "label1,label2")
                .addFormDataPart("video_url", "http://xxx.xxx")
                .addFormDataPart("is_aigc", "true")
                .build();

        Request request = new Request.Builder()
                .url("http://localhost:8080/revice/image")
                .post(requestBody)
                .build();
        try (Response response = client.newCall(request).execute()) {
            return response.body().string();
        }
    }
```