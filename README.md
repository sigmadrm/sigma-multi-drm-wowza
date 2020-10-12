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
    | sm_asset_pattern_default        | Root/Application | Bool   | Optional    | Sử dụng pattern mặc định để tạo ra tên nội dung. Mặc định là true |
    | sm_asset_pattern                | Root/Application | String | Optional    | Pattern để sinh ra tên nội dung. Lấy group thứ nhất để làm tên nội dung. Trường này sẽ bị bỏ qua nếu sm_asset_pattern_default là true. |
    | **cupertinoEncryptionAPIBased** | Root/Application | Bool   | Cho phép mã hóa fairplay. Cái đặt giá trị này là true |
  
    

## 3. Cách tạo ra tên của nội dung trong hệ thống

Tên của nội dung trong hệ thống được tạo ra theo cú pháp dưới đây:

<Tên_ứng_dụng_trong_wowza>__<Tên_kênh_hoặc_vod>
## 4. Cách đặt tên các stream để tạo ra multi profile

### 4.1. Mặc định

Mặc định sẽ không cần cấu hình 2 trường **sm_asset_pattern_default** và **sm_asset_pattern**. Chỉ cần đặt tên các stream và các file vod theo cấu trúc sau:

{ASSET_NAME}_{PROFILE}.[stream|mp4]

**Ví dụ:**

Nếu tên stream là **bigbuckbunny_200000.mp4** thì tên của nội dung sẽ là **bigbuckbunny**.

### **4.2. Custom**

Để sử dụng phần này, bạn sẽ cần một chút hiểu biết về Regular Expression. Chúng tôi đề xuất bạn nên sử dụng cách 4.1 để tạo ra profile một cách dễ dàng hơn.

- Cài đặt trường **sm_asset_pattern_default** là fail.
- Cài đặt trường **sm_asset_pattern** là một biểu thức chính quy để bỏ qua phần thông số của profile.

**Ví dụ:**

Tên nội dung của bạn là **cdntest_vtv1-0.stream**, thì trường **sm_asset_pattern** có giá trị là **(.\*)-(.\*).stream**. Khi đó thì tên nội dung của bạn sẽ là **cdntest_vtv1**.
