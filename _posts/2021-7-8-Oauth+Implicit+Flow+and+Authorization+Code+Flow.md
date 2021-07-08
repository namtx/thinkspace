---
layout: post
label: til
title: "Oauth Implicit Flow and Authorization Code Flow"
---

<p>
  
</p>
Implicit flow đc dùng trong trường hợp muốn lấy short-lived token ở public client ví dụ SPA, mobile app. Token đó sẽ bị expired sau một thời gian ngắn và user phải đăng nhập lại để lấy token mới. Điều này đơn giản vì client đó không an toàn, người khác có thể dễ dàng truy cập, can thiệp để lấy client secret nếu có.
Authorization code flow sẽ có khả năng lấy đc refresh token cho phép truy cập dài hạn, nên chỉ dùng khi nào làm confidential client, có khả năng bảo về client secret ví dụ là client ở backend. Sau khi đc cấp quyền thì nhờ vào refresh token, backend có thể duy trì truy cập đó rất lâu trước khi người dùng bỏ quyền của client đó.
Còn về phần token hay code nằm trên URL hay không thì cái đó còn phụ thuộc vào response mode do client lựa chọn, chứ không phải là do flow. Chẳng qua mặc định thì authorization code flow dùng form POST nên nó sẽ an toàn hơn. Dĩ nhiên là public client không phải backend thì không thể sử dụng form POST rồi vì không có backend xử lý và phải dựa vào tham số trên URL, nên điều này là không thể tránh khỏi.

Source: [Webuild Community](https://webuild.substack.com/)

