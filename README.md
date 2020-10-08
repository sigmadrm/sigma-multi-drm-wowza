# **Hướng dẫn tích hợp Sigma MultiDRM vào hệ thống Wowza**

Tài liệu này sẽ hướng dẫn tích hợp hệ thống Sigma Multi DRM vào hệ thống Wowza packager.

## **1. Yêu cầu môi trường**

- Wowza: 4.7.x+
- SigmaDRM Plugin: 1.0.0+
- Đảm bảo wowza server có thể kết nối internet và có thể truy nhập vào các địa chỉ có domain là *.sigmadrm.com.

## **2. Hướng dẫn tích hợp:**

- **Bước 1:** Tạo một ứng dụng trong wowza. 

  ![wowza_createapp](https://dashboard.sigmadrm.com/assets/wowza_createapp.png)
  

- **Bước 2:** Chỉ cho phép các kiểu playback: MPEG-DASH, Apple HLS, Microsoft Smooth Streaming.

  - Chọn Application.
  - Chọn ứng dụng cần edit.
  - Chọn Edit
  
  ![wowza_enable_playback](https://dashboard.sigmadrm.com/assets/wowza_enable_playback.png)
  

- **Bước 3:** Copy file .jar vào trong thư mục lib của wowza "[WOWZA_INSTALL_DIR]/lib".

- **Bước 4:** Cấu hình các tham số cho wowza.

  - **Bước 4.1:** Cấu hình library cho ứng dụng: Application -> Modules -> Edit:

    ![wowza_config_lib](https://dashboard.sigmadrm.com/assets/wowza_config_lib.png)

  - **Bước 4.2:** Cấu hình properties: Properties ->HTTP Streamers Cupertino Settings

    ![wowza_config_lib](https://dashboard.sigmadrm.com/assets/wowza_hls_version.png)

  - **Bước 4.2:** Cấu hình properties: Properties -> Edit -> Custom
  
    ![wowza_config_lib](https://dashboard.sigmadrm.com/assets/wowza_config_custom_property.png)
  
  
    | Tên thuộc tính                  | Đường dẫn        | Kiểu   | Mô tả                                                 |
    | ------------------------------- | ---------------- | ------ | ----------------------------------------------------- |
    | **sm_merchant**                 | Root/Application | String | Merchant id của khách hàng                            |
    | sm_app_id                       | Root/Application | String | App id của khách hàng                                 |
    | sm_username                     | Root/Application | String | Tài khoản đăng nhập vào hệ thống của khách hàng       |
    | sm_password                     | Root/Application | String | Mật khẩu đăng nhập của tài khoản                      |
    | sm_env                          | Root/Application | Int    | Môi trường phát triển: Dùng thử: 1, Dùng thật: 2      |
    | **cupertinoEncryptionAPIBased** | Root/Application | Bool   | Cho phép mã hóa fairplay. Cái đặt giá trị này là true |
  
    

## 3. Cách tạo ra tên của nội dung trong hệ thống

Tên của nội dung trong hệ thống được tạo ra theo cú pháp dưới đây:

<Tên_ứng_dụng_trong_wowza>__<Tên_kênh_hoặc_vod>
