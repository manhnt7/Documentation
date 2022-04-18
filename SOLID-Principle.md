# 1. SRP: The Single-Responsibility Principle

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/SRP-image.png)

Hãy xem xét trò chơi bowling từ Chương 6. Đối với hầu hết quá trình phát triển của nó, lớp Game đã xử lý 2 nhiệm vụ riêng biệt. Theo dõi khung hình hiện tại và tính toán điểm số. Cuối cùng, RCM và RSK đã tách hai nhiệm vụ này thành hai lớp. Gồm có class Game và Scorer, Game thực hiện theo dõi các chỉ số, Scorer có nhiệm vụ tính toán điểm số.

Tại sao phải tách hai nhiệm vụ này thành các lớp riêng biệt là một điều quan trọng nên làm? Bởi vì mỗi nhiệm vụ là một trục của sự thay đổi. Khi yêu cầu được thay đổi, sự thay đổi ấy sẽ tác động lên nhiệm vụ của từng lớp. 
Nếu một class có nhiều hơn 1 nhiệm vụ, sau đó các nhiệm vụ đó đi với nhau. Những thay đổi đối với một trách nhiệm có thể làm giảm hoặc hạn chế khả năng của lớp để đáp ứng các trách nhiệm khác. Loại khớp nối này dẫn đến các thiết kế mỏng manh, dễ vỡ ra khi tiến hành thay đổi.

Với ví dụ dưới, Rectangle class có 2 method, một là dùng để vẽ hình chữ nhật, 1 là dùng để tính toán diện tích.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-8-1.png)

Hai ứng dụng khác nhau sử dụng lớp Rectangle. Một ứng dụng tính toán hình học, nó sử dụng lớp Rectangle để tính toán các hình dạng về hình học. Nó không có chức năng vẽ hình dạng hình học. Một ứng dụng khác là một ứng dụng đồ họa. Nó cũng có thể thực hiện một số tính toán hình học, nhưng nó chắc chắn nó có chức năng vẽ hình chữ nhật.

Thiết kế này vi phạm Single-Responsibility Principle (SRP). 
Vi phạm này gây ra một số vấn đề khó chịu. Trong ví dụ này. chúng ta phải thêm GUI vào trong ứng dụng tính toán ấy. Nếu  đây là một ứng dụng được build bằng C ++, GUI sẽ phải được liên kết, tốn thời gian liên kết, thời gian biên dịch và dung lượng bộ nhớ. Ứng dụng được xây dựng bằng Java, các tệp .class cho GUI phải deploy theo. Cuối cùng dẫn đến dư thừa lớn.

Thứ hai, thay đổi Rectangle để đáp ứng thay đổi từ GraphicalApplication thì khiến chúng ta phải xây dựng lại, kiểm tra lại và deploy lại ComputationalGeometryApplication. Nếu chúng ta quên làm điều này, ứng dụng đó có thể bị hỏng theo những cách không thể đoán trước.
Thiết kế tốt hơn là tách hai nhiệm vụ thành hai lớp hoàn toàn khác nhau như thể hiện trong Hình 8-2. Thiết kế này chuyển các phần tính toán của Rectangle vào lớp GeometricRectangle. Bây giờ
những thay đổi được thực hiện đối với cách hiển thị hình chữ nhật không thể ảnh hưởng đến ComputationalGeometryApplication.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-8-2.png)

Responsibility là gì?
Trong ngữ cảnh SRP, chúng ta định nghĩa thêm một responsibility là chúng ta đã thêm “một lý do để thay đổi”. Nếu bạn suy nghĩ sẽ thay đổi một lớp, sau đó lớp đó có hơn nhiều responsibility. Điều này đôi khi khó thể nhận ra. Chúng ta đã quen việc nhóm các responsibility. Ví dụ, với interface Modem. Hầu hết chúng ta sẽ đồng ý rằng giao diện này trông hoàn toàn hợp lý. Bốn hàm mà nó khai báo chắc chắn là các hàm thuộc về một modem.

*Listing 8-1*
***Modem.java -- SRP Violation***
```java
{
 public void dial(String pno);
 public void hangup();
 public void send(char c);
 public char recv();
}
```
Tuy nhiên, ở đây có tới 2 responsibilities tồn tại. Đó là quản lý về kết nối và giao tiếp dữ liệu. dial và hangup là 2 hàm có nhiệm vụ kết nối với modem, trong khi send và recv là 2 hàm có chức năng thực hiện giao tiếp dữ liệu.

Có nên tách 2 responsibility này không? Điều đó phụ thuộc vào cách thay đổi của ứng dụng. Nếu ứng dụng thay đổi các chức năng kết nối, thì sau đó design sẽ có mùi của Rigidity bởi vì class sẽ call send và recv và thực hiện recompiled và redeployed nhiều hơn hơn chúng ta muốn. Trong trường hợp đó, hai trách nhiệm nên được tách biệt như trong Hình 8-3.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-8-3.png)

Mặt khác, ứng dụng không cần tách chúng nếu sự thay đổi của 2 responsibilities thực hiện cùng một thời điểm. Thật vậy, tách chúng ra sẽ có mùi “Không cần thiết - Sự phức tạp”.

Tách Coupled Responsibilities
Lưu ý rằng trong Hình 8-3, tôi đã giữ cả hai trách nhiệm được kết hợp trong lớp ModemImplementation.  This is not desirable, but it may be necessary. Thường có những lý do, liên quan đến các chi tiết của phần cứng hoặc hệ điều hành, buộc chúng ta phải kết hợp những thứ mà chúng ta không muốn kết hợp.
Chúng ta có thể nhìn lớp ModemImplementation như một cục bùn, hoặc một mụn cóc; tuy nhiên, hãy lưu ý rằng tất cả phụ thuộc đều đi qua nó. Không ai ngoại trừ main cần biết rằng nó tồn tại.

Persistence - Sự bền bỉ
Hình 8-4 cho thấy một vi phạm phổ biến đối với SRP. Lớp Employee bao gồm business rules và persistence control. Hai trách nhiệm này hầu như không bao giờ được trộn lẫn. Business rules có khuynh hướng thay đổi thường xuyên và persitence có thể không thay đổi thường xuyên, nó thay đổi vì những lý do hoàn toàn khác nhau. Cho nên sự ràng buộc giữa business và persitence sẽ gây ra những rắc rối.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-8-4.png)

Tóm lại
SRP là một trong những nguyên tắc đơn giản nhất và là một trong những nguyên tắc khó thực hiện đúng. Các responsibility liên đới là một cái gì đó mà chúng tôi làm một cách tự nhiên. Tìm kiếm và tách biệt những responsibility đó với nhau là chiếm phần lớn của việc thiết kế phần mềm. 
Chúng ta mới chỉ áp dụng một quy tắc đầu tiên của SOLID – quy tắc đơn nhiệm – vào thiết kế phần mềm thì code đã trở nên trong sáng và hữu dụng hơn rất nhiều. Có thể dễ dàng hình dung được, nếu các anh em developer chúng ta đều nắm vững và áp dụng các quy tắc này, thì phần mềm của chúng ta sẽ tốt lên rất nhiều.
Phần còn lại của các nguyên tắc chúng ta sẽ thảo luận quay lại vấn đề này theo cách này hay cách khác. 

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/OCP-image.png)

# 2. OCP: The Open–Closed Principle

Các thực thể phần mềm (classes, modules, functions, v.v.) nên được mở để mở rộng, nhưng sửa đổi thì không. 
Khi một thay đổi dẫn đến một loạt các thay đổi đối với các module phụ thuộc, lúc đó design sẽ có mùi Rigidity. OCP khuyên chúng tôi nên cấu trúc lại hệ thống để những thay đổi tiếp theo sẽ không gây ra nhiều sửa đổi hơn.
