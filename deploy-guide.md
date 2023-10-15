## 1. Đăng nhập vào VPS

## 2. Cài đặt tường lửa
   - Xem danh sách các ứng dụng được hỗ trợ bởi UFW:
     ```
     sudo ufw app list
     ```
     - Kết quả sẽ hiển thị:
     ```
     Available applications
         OpenSSH
     ```

   - OpenSSH là một công cụ cho phép đăng nhập từ xa bằng giao thức SSH. Điều này cần thiết để đăng nhập vào máy chủ qua SSH. Bạn cần cho phép kết nối SSH bằng cách nhập lệnh:
     ```
     sudo ufw allow OpenSSH
     ```

   - Sau đó, bạn cần áp dụng các thay đổi. Để kích hoạt tường lửa, nhập lệnh sau:
     ```
     sudo ufw enable
     ```
     - Nhấn "y" và sau đó "Enter" để tiếp tục.

   - Để kiểm tra tường lửa đã được kích hoạt và thay đổi đã được áp dụng, kiểm tra trạng thái của tường lửa bằng lệnh:
     ```
     sudo ufw status
     ```
     - Kết quả sẽ hiển thị:
     ```
     Status: active

     To                         Action      From
     --                         ------      ----
     OpenSSH                    ALLOW       Anywhere
     OpenSSH (v6)               ALLOW       Anywhere (v6)
     ```

## 3. Cài đặt Nginx
   1. Cài đặt Nginx
      - Trong cửa sổ terminal, đảm bảo rằng kết nối SSH đến máy chủ của bạn không bị gián đoạn. Sau đó chạy lệnh sau để cài đặt Nginx:
        ```
        sudo apt-get install nginx
        ```

   2. Kích hoạt Nginx trong UFW
      - Tương tự như việc bạn đã kích hoạt OpenSSH để cho phép kết nối SSH. Bắt đầu bằng việc liệt kê tất cả các ứng dụng bằng cách chạy:
        ```
        sudo ufw app list
        ```
        - Kết quả sẽ hiển thị:
        ```
            Available applications:
                Nginx Full
                Nginx HTTP
                Nginx HTTPS
                OpenSSH
        ```
      - Since we haven't set up Let's Encrypt, we will temporarily select "Nginx HTTP".
        ```
            sudo ufw allow 'Nginx HTTP'
        ```
      - After allowing traffic for "Nginx HTTP", you can verify that the change was successful by running:
        ```
            sudo ufw status
        ```
        - Kết quả sẽ hiển thị:
        ```
            Status: active

            To                         Action      From
            --                         ------      ----
            OpenSSH                    ALLOW       Anywhere
            Nginx HTTP                 ALLOW       Anywhere
            OpenSSH (v6)               ALLOW       Anywhere (v6)
            Nginx HTTP (v6)            ALLOW       Anywhere (v6)
        ```
   3. Cấu hình Nginx Reverse Proxy
      - Mở file /etc/nginx/nginx.conf và kiểm tra xem đã có dòng include này chưa:
       ```
        ...
            ...
            include /etc/nginx/conf.d/*.conf;
        ...
       ```
      - Tạo một file my-app.conf trong thư mục /etc/nginx/conf.d. Ở đây mình sử dụng trình editor vi.
       ```
        vi /etc/nginx/conf.d/my-app.conf
       ```
      - Copy nội dung từ nginx.conf sang my-app.conf
   