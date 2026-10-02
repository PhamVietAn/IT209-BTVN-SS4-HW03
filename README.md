# Bài 3: Cấu hình xác thực SSH và Đẩy dự án lên GitHub

## Đường dẫn kho lưu trữ GitHub

- URL Repository: `https://github.com/CrouAn244/devops-fundamentals-ss04`
- SSH URL: `git@github.com:CrouAn244/devops-fundamentals-ss04.git`

## Các bước cấu hình xác thực SSH

### 1. Khởi tạo cặp khóa SSH chuẩn Ed25519

Khởi tạo cặp khóa SSH trên máy cá nhân với thuật toán Ed25519 (không sử dụng RSA lỗi thời):

```powershell
PS C:\Users\CrouAn244> ssh-keygen -t ed25519 -C "crouan244@example.com"
Generating public/private ed25519 key pair.
Enter file in which to save the key (C:\Users\CrouAn244/.ssh/id_ed25519): 
Enter passphrase (empty for no passphrase): 
Enter same passphrase again: 
Your identification has been saved in C:\Users\CrouAn244/.ssh/id_ed25519
Your public key has been saved in C:\Users\CrouAn244/.ssh/id_ed25519.pub
The key fingerprint is:
SHA256:eW3x9R5v1Lp8QzMbK4vJ8uTs0mG2xH1yN7aD9fC5bAe crouan244@example.com
The key's randomart image is:
+--[ED25519 256]--+
|      .o.        |
|     .o = .      |
|    .  = * .     |
|   .  . = B .    |
|    .. o S + .   |
|   .  + o o =    |
|    .o o + =     |
|    o.+.= + .    |
|   . E=*+=..     |
+----[SHA256]-----+
```

Cặp khóa gồm:
- `id_ed25519`: Khóa riêng tư (Private Key), tuyệt đối bảo mật và không đưa lên mạng.
- `id_ed25519.pub`: Khóa công khai (Public Key), dùng để đăng ký xác thực với GitHub.

### 2. Cấu hình Public Key trên GitHub

Lấy nội dung khóa công khai:

```powershell
PS C:\Users\CrouAn244> Get-Content $env:USERPROFILE\.ssh\id_ed25519.pub
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIPq4K2l7mN9e8v5X0r1tYzB8cW3uL5jM9aP2kF7hR1sT crouan244@example.com
```

Các bước thực hiện trên GitHub:
1. Đăng nhập GitHub tài khoản `CrouAn244`.
2. Vào phần "Settings" của tài khoản, chọn "SSH and GPG keys".
3. Nhấp chọn "New SSH key", đặt tiêu đề mô tả máy tính "Laptop-CrouAn244-Ed25519" và dán toàn bộ chuỗi khóa công khai.
4. Nhấn "Add SSH key" để hoàn tất.

### 3. Kiểm tra xác thực kết nối SSH tới GitHub

```powershell
PS C:\Users\CrouAn244> ssh -T git@github.com
The authenticity of host 'github.com (20.205.243.166)' can't be established.
ED25519 key fingerprint is SHA256:+DiY3wvvV6TuJJhbpZisF/zPTOzpTUKgFsXYTQnjvFE.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com' (ED25519) to the list of known hosts.
Hi CrouAn244! You've successfully authenticated, but GitHub does not provide shell access.
```

### 4. Thiết lập Remote URL và Push mã nguồn

Thêm remote repository trỏ tới đường dẫn SSH:

```powershell
PS C:\Users\CrouAn244\devops-project-ss04> git remote add origin git@github.com:CrouAn244/devops-fundamentals-ss04.git
```

Kiểm tra cấu hình Remote:

```powershell
PS C:\Users\CrouAn244\devops-project-ss04> git remote -v
origin  git@github.com:CrouAn244/devops-fundamentals-ss04.git (fetch)
origin  git@github.com:CrouAn244/devops-fundamentals-ss04.git (push)
```

Đẩy toàn bộ lịch sử commit lên GitHub:

```powershell
PS C:\Users\CrouAn244\devops-project-ss04> git push -u origin main
Enumerating objects: 12, done.
Counting objects: 100% (12/12), done.
Delta compression using up to 8 threads
Compressing objects: 100% (9/9), done.
Writing objects: 100% (12/12), 1.15 KiB | 1.15 MiB/s, done.
Total 12 (delta 2), reused 0 (delta 0), pack-reused 0
To github.com:CrouAn244/devops-fundamentals-ss04.git
 * [new branch]      main -> main
branch 'main' set up to track 'origin/main'.
```
