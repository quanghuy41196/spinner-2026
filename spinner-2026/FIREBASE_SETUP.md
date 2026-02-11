# 🔥 Hướng dẫn Setup Firebase cho Spinner

## 📋 Tổng quan

Ứng dụng đã được tích hợp Firebase Realtime Database để đồng bộ dữ liệu giữa các máy. Bây giờ bạn có thể:
- ✅ Cập nhật danh sách từ trang Admin → Tự động sync đến máy Spinner
- ✅ Thay đổi guaranteed winners → Sync real-time
- ✅ Nhiều màn hình hiển thị cùng lúc, luôn đồng bộ

---

## 🚀 Các bước setup

### Bước 1: Tạo Firebase Project

1. Truy cập: https://console.firebase.google.com/
2. Click **"Add project"** (Thêm dự án)
3. Đặt tên project (ví dụ: "spinner-app")
4. Tắt Google Analytics (nếu không cần)
5. Click **"Create project"**

### Bước 2: Tạo Realtime Database

1. Trong Firebase Console, vào **"Build" > "Realtime Database"**
2. Click **"Create Database"**
3. Chọn location gần nhất (ví dụ: `asia-southeast1` cho VN)
4. Chọn **"Start in test mode"** (để bắt đầu nhanh)
   - ⚠️ **Lưu ý:** Test mode cho phép mọi người đọc/ghi. Sau 30 ngày cần cập nhật rules!
5. Click **"Enable"**

### Bước 3: Lấy Firebase Config

1. Trong Firebase Console, click vào ⚙️ **Settings** > **Project settings**
2. Scroll xuống phần **"Your apps"**
3. Click vào icon **</>** (Web)
4. Đặt tên app (ví dụ: "Spinner Web App")
5. ✅ Check **"Also set up Firebase Hosting"** (nếu muốn)
6. Click **"Register app"**
7. Copy toàn bộ config (phần trong `firebaseConfig`)

### Bước 4: Cấu hình trong Project

1. Copy file `.env.example` thành `.env`:
   \`\`\`bash
   cp .env.example .env
   \`\`\`

2. Mở file `.env` và điền thông tin từ Firebase:
   \`\`\`
   VITE_FIREBASE_API_KEY=AIzaSy...
   VITE_FIREBASE_AUTH_DOMAIN=your-project.firebaseapp.com
   VITE_FIREBASE_DATABASE_URL=https://your-project-default-rtdb.firebaseio.com
   VITE_FIREBASE_PROJECT_ID=your-project
   VITE_FIREBASE_STORAGE_BUCKET=your-project.appspot.com
   VITE_FIREBASE_MESSAGING_SENDER_ID=123456789
   VITE_FIREBASE_APP_ID=1:123456789:web:abc123
   \`\`\`

### Bước 5: Deploy lên Vercel

1. Push code lên GitHub (nếu chưa có):
   \`\`\`bash
   git add .
   git commit -m "Add Firebase integration"
   git push
   \`\`\`

2. Trong Vercel Project Settings:
   - Vào **Settings > Environment Variables**
   - Thêm từng biến môi trường từ file `.env`
   - ✅ Check cả 3 môi trường: **Production, Preview, Development**

3. Redeploy project:
   - Vào **Deployments**
   - Click **"..."** ở deployment mới nhất
   - Chọn **"Redeploy"**

---

## 🔒 Cập nhật Security Rules (Quan trọng!)

Sau khi test thành công, cập nhật Firebase Realtime Database Rules:

1. Vào **"Realtime Database" > "Rules"**
2. Thay thế rules hiện tại:

\`\`\`json
{
  "rules": {
    "spinner_data": {
      ".read": true,
      ".write": true
    }
  }
}
\`\`\`

**Giải thích:**
- Cho phép mọi người đọc/ghi vào `spinner_data` 
- ⚠️ Chỉ dùng cho internal app, không public
- Nếu cần bảo mật cao hơn, xem phần **Security nâng cao** bên dưới

---

## ✅ Test thử

1. Mở 2 tab trình duyệt:
   - Tab 1: Trang Admin (`/admin`)
   - Tab 2: Trang Spinner (`/`)

2. Tại Tab 1 (Admin):
   - Login vào Admin
   - Thêm guaranteed winner mới

3. Tại Tab 2 (Spinner):
   - Mở Settings modal
   - Kiểm tra danh sách → Sẽ tự động cập nhật! ✨

---

## 🛡️ Security nâng cao (Optional)

Nếu muốn bảo mật chặt hơn, dùng rules này:

\`\`\`json
{
  "rules": {
    "spinner_data": {
      ".read": true,
      ".write": "auth != null"  // Chỉ cho phép user đã đăng nhập
    }
  }
}
\`\`\`

Sau đó cần implement Firebase Authentication (Email/Password hoặc Google Sign-In).

---

## 🐛 Troubleshooting

### Lỗi: "Permission denied"
- Kiểm tra Firebase Rules
- Đảm bảo `databaseURL` đúng trong config

### Data không sync
- Mở DevTools > Console, xem lỗi
- Kiểm tra Firebase Console > Realtime Database > Data
- Đảm bảo có internet connection

### Lỗi: "Firebase: Error (auth/...)"
- Không cần Authentication cho bản hiện tại
- Nếu thấy lỗi auth, có thể bỏ qua

---

## 📱 Sử dụng thực tế

### Kịch bản 1: Admin ở laptop, Spinner ở TV
1. Laptop: Mở `/admin`, cấu hình guaranteed winners
2. TV: Mở `/`, chạy vòng quay
3. Mọi thay đổi từ Admin → Tự động sync đến TV

### Kịch bản 2: Nhiều màn hình hiển thị
1. Màn 1: Chạy spinner
2. Màn 2, 3, 4: Hiển thị kết quả
3. Tất cả đều sync real-time từ Firebase

---

## 💡 Tips

- File `.env` **KHÔNG** được push lên Git (đã có trong `.gitignore`)
- Trên Vercel, nhớ thêm biến môi trường vào **Settings**
- Firebase Free tier: 1GB storage, 10GB/tháng bandwidth → Đủ dùng!

---

## 🎉 Hoàn tất!

Bây giờ app của bạn đã có khả năng đồng bộ real-time qua Firebase! 🚀

Nếu có vấn đề gì, check:
1. Firebase Console > Realtime Database > Data (xem data có update không)
2. Browser DevTools > Console (xem error logs)
3. Vercel > Deployments > Logs (xem deployment logs)
