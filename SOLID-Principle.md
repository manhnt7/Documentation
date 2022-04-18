# 1. Nguyên tắc một trách nhiệm - Single Responsibility Principle
`Một class nên chỉ có một lý do để thay đổi`

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/SRP-image.png)

Hãy xem xét trò chơi bowling từ Chương 6. Đối với hầu hết quá trình phát triển của nó, class Game đã xử lý 2 nhiệm vụ riêng biệt. Nó cần lưu lại trạng thái hiện tại của trò chơi, cũng như tính toán điểm số. Cuối cùng, RCM và RSK tách nó thành 2 class với 2 trách nhiệm khác nhau. Class Game đóng vai trò lưu trữ trạng thái của trò chơi, và class Score chịu trách nhiệm tính toán điểm số.

Tại sao phải tách hai nhiệm vụ này thành các lớp riêng biệt là một điều quan trọng nên làm?  Bởi mỗi trách nhiệm có một trục quản lý thay đổi khác nhau. Khi yêu cầu thay đổi, hiển nhiên là có thay đổi đến trách nhiệm của một trong số các class. Nếu một class có nhiều hơn một trách nhiệm, sẽ có nhiều hơn một lý do để nó phải thay đổi.

Nếu một class có nhiều hơn 1 nhiệm vụ, sau đó các nhiệm vụ đó đi với nhau. Những thay đổi đối với một trách nhiệm có thể làm giảm hoặc hạn chế khả năng của lớp để đáp ứng các trách nhiệm khác. Loại khớp nối này dẫn đến các thiết kế mỏng manh, dễ vỡ ra khi tiến hành thay đổi.

Với ví dụ dưới, Rectangle class có 2 method, một là dùng để vẽ hình chữ nhật, 1 là dùng để tính toán diện tích.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-8-1.png)

Hai ứng dụng khác nhau sử dụng lớp Rectangle. Một ứng dụng tính toán hình học, nó sử dụng lớp Rectangle để tính toán các hình dạng về hình học. Nó không có chức năng vẽ hình dạng hình học. Một ứng dụng khác là một ứng dụng đồ họa. Nó cũng có thể thực hiện một số tính toán hình học, nhưng nó chắc chắn nó có chức năng vẽ hình chữ nhật.

Thiết kế này vi phạm Single-Responsibility Principle (SRP). 
Vi phạm này gây ra một số vấn đề khó chịu. Trong ví dụ này. chúng ta phải thêm thư viện GUI vào trong ứng dụng tính toán ấy. Nếu  đây là một ứng dụng được build bằng C ++, GUI sẽ phải được liên kết, tốn thời gian liên kết, thời gian biên dịch và dung lượng bộ nhớ. Ứng dụng được xây dựng bằng Java,  tập tin .class cho GUI cần được đẩy lên nền tảng chạy ứng dụng.

Thứ hai, thay đổi Rectangle để đáp ứng thay đổi từ GraphicalApplication thì khiến chúng ta phải xây dựng lại, kiểm tra lại và deploy lại ComputationalGeometryApplication. Nếu chúng ta quên làm điều này, ứng dụng đó có thể bị hỏng theo những cách không thể đoán trước.

Một thiết kế tốt hơn có được khi chúng ta tách hai trách nhiệm ra hai class khác nhau như hình 8-2. Thiết kế này đẩy phần tính toán ra class GeometricRectangle. Lúc này, thay đổi về việc hiển thị hình chữ nhật không hề ảnh hưởng đến ứng dụng ComputationalGeometryApplication.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-8-2.png)

**Trách nhiệm là gì?**

Khi nói đến SRP, chúng ta định nghĩa trách nhiệm là lý do để thay đổi. Nếu bạn có thể nghĩ đến nhiều hơn một động cơ để thay đổi class, nghĩa là class đó có nhiều hơn một trách nhiệm. Điều này đôi khi rất khó để nhận ra. Chúng ta thường nghĩ về trách nhiệm trong một nhóm. Ví dụ, xem xét lớp Modem dưới đây. Hầu hết chúng ta đều đồng ý rằng lớp này nhìn có vẻ rất ổn. Cả bốn hàm đều được định nghĩa rõ ràng với các chứng năng liên quan đến một mô-đem.

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

Tuy nhiên, theo một cách khác, nếu ứng dụng thay đổi mà không khiến các trách nhiệm phải thay đổi thường xuyên, chúng ta sẽ không cần phải tách class ra. Khi đó, việc tách biệt chúng sẽ dẫn đến sự phức tạp không cần thiết (Needless Complexity).

Cần có một sự bình tĩnh ở đây. Một trục của sự thay đổi chỉ thực sự là một trục của sự thay đổi nếu thay đổi thực sự diễn ra. Sẽ không khôn khéo nếu áp dụng SRP hoặc bất cứ nguyên tắc nào nếu không có bất kỳ triệu chứng nào cần được sửa chữa.

**Tách biệt các chức năng bị liên kết**

Lưu ý rằng trong Hình 8-3, chúng ta đã liên kết 2 chức năng trong một class ModemImplementation. Đó không phải là những gì chúng ta thực sự muốn, nhưng nó là cần thiết. Đôi khi có những lý do, như là việc tương thích với phần cứng hay hệ điều hành, buộc chúng ta liên kết các thành phần lại chứ không phải tách biệt chúng ra. Tuy nhiên bằng cách tách biệt các lớp ra, chúng ta đã tách biệt các khái niệm khác nhau trong ứng dụng của mình.

Chúng ta có coi ModemImplementation là một thiết kế không hoàn chỉnh, tuy nhiên, nên nhớ rằng tất cả các thành phần phụ thuộc đã được tách biệt khỏi nó. Không ai cần phụ thuộc vào class đó nữa. Không ai ngoại trừ chương trình chính (main) biết là nó tồn tại. Do vậy, chúng ta đã để sự xấu xí đằng sau hàng rào bảo vệ, sự xấu xí đó không lọt ra ngoài và không làm xấu đi các thành phần khác.

**Persistence - Sự bền bỉ**

Hình 8-4 cho thấy ví dụ phổ biến của việc vi phạm SRP. Class Employee bao gồm cả logic về nghiệp vụ và quản lý việc lưu trữ. Hai trách nhiệm này gần như không bao giờ nên được ghép vào một class. Logic về nghiệp vụ có xu hướng thay đổi liên tục, trong khi công việc lưu trữ thì ít khi thay đổi, và thường thay đổi ví một lý do hoàn toàn khác. Đẩy logic nghiệp vụ cùng class với công việc lưu trữ có thể gây ra những rắc rối.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-8-4.png)

Thật may mắn, như đã đề cập ở chương 4, bằng việc áp dụng phương pháp phát triển địn hướng bởi kiểm thử (test-driven development), chúng ta thường buộc hai trách nhiệm này tách biệt trong hai class khác nhau. Tuy nhiên, trong trường hợp bài kiểm thử không buộc việc tách biệt được thực hiện, chương trình sẽ có mùi cứng nhắc (Rigidity) và dễ vỡ (Fragility), chương trình cần được tái cấu trúc sử dụng mẫu thiết kế FACADE và PROXY.

**Tóm lại**

SRP là một trong những nguyên tắc đơn giản nhất và là một trong những nguyên tắc khó thực hiện đúng. Các responsibility liên đới là một cái gì đó mà chúng tôi làm một cách tự nhiên. Tìm kiếm và tách biệt những responsibility đó với nhau là chiếm phần lớn của việc thiết kế phần mềm. 
Chúng ta mới chỉ áp dụng một quy tắc đầu tiên của SOLID – quy tắc đơn nhiệm – vào thiết kế phần mềm thì code đã trở nên trong sáng và hữu dụng hơn rất nhiều. Có thể dễ dàng hình dung được, nếu các anh em developer chúng ta đều nắm vững và áp dụng các quy tắc này, thì phần mềm của chúng ta sẽ tốt lên rất nhiều.
Những nguyên tắc được nhắc đến tiếp theo sẽ thảo luận lại vấn đề này theo một cách khác. 

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/OCP-Image.png)

# 2. OCP: The Open–Closed Principle

`Các thành phần của phần mềm (class, module, hàm,...) cần mở cho sự mở rộng nhưng đóng với sự thay đổi`

Khi một thay đổi dẫn đến một loạt các thay đổi đối với các module phụ thuộc, lúc đó design sẽ có mùi cứng nhắc - Rigidity. OCP khuyên chúng tôi nên cấu trúc lại chương trình để những thay đổi tiếp theo sẽ không gây ra nhiều sửa đổi trong chương trình. Nếu OCP được thực hiện đúng, những thay đổi kiểu này sẽ đạt được bằng việc viết thêm những dòng code, không phải thay đổi những dòng code cũ đã hoạt động tốt.

Đây có vẻ là mục tiêu xa vời - một mục tiêu vàng gần như không thể đạt được - tuy nhiên, thực tế có khá nhiều chiến lược đơn giản và hiệu quả giúp đạt được lý tưởng này.

**Mô tả**

Một module thỏa nguyên tắc đóng-mở sẽ có hai tính chất. Chúng là:

1. "Mở cho sự mở rộng" - "Open for extension"
   Điều đó có nghĩa là hành vi của module có thể mở rộng. Khi yêu cầu hệ thống thay đổi, chúng ta có thể mở rộng module để có thể đáp ứng các hành vi mới thoả mãn yêu cầu. Nói một cách khác, chúng ta có thể thay đổi cách mà module làm việc.

2. "Đóng với sự thay đổi" - "Closed for modification"
   Mở rộng module không kéo theo việc thay đổi mã nguồn của module. Tập tin chứa mã nhị phân đóng vai trò thực thi của module, hoặc là các thư viện liên kết, DLL hay Java .jar đều không bị đụng chạm đến.

Có vẻ hai tính chất này đang mâu thuẫn và trái ngược nhau. Các thông thường để mở rộng hành vi của module là thay đổi nó. Một module không bị thay đổi thông thường đã có hành vi cố định.

Làm cách nào để một module có thể được thay đổi mà không thay đổi mã nguồn của nó? Làm cách nào để thay đổi cách mà module làm việc, nhưng không thay đổi module?

**Trừu tượng là chìa khoá**

Trong C++, Java hay bất cứ ngôn ngữ lập trình hướng đối tượng nào (OOPL), chúng ta có thể trừu tượng hóa các hành vi quy định ngay từ trước trong một nhóm không giới hạn các hành vi có thể. Sự trừu tượng thể hiện ở class trừu tượng cơ sở, trong khi một lương không giới hạn các hành vi có thể được thể hiện ở các class thừa kế từ class cơ sở.

Chúng ta hoàn toàn có thể điều khiển một module dưới hình thức trừu tượng. Những module như vậy có thể đóng với sự thay đổi bởi chúng phụ thuộc vào một lớp trừu tượng đã được cố định. Tuy nhiên, hành vi của module đó có thể mở rộng bằng cách tạo ra class phát sinh từ lớp trừu tượng.

Hình 9-1 mô tả một thiết kế đơn giản không thỏa mãn OCP. Cả class Client và Server đều rời rạc. Client sử dụng class Server. Nếu chúng ta mong chờ đối tượng Client sử dụng các đối tượng Server khác nhau, class Client cần thay đổi để gọi tên class Server mới.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-9-1.png)

Hình 9-2 cho thấy một thiết kế phù hợp, thỏa mãn OCP. Trong trường hợp này, class ClientInterface là một lớp trừu tượng với phương thức trừu tượng. Class Client sử dụng tính trừu tượng này, tuy nhiên đối tượng của class Client cần sử dụng đối tượng được sinh ra từ class Server. Nếu chúng ta muốn đối tượng của class Client sử dụng đối tượng khác của class Server, khi đó một phiên bản khác sinh ra từ ClientInterface có thể được tạo ra. Do vậy, class Client có thể giữ không đổi.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-9-2.png)

Class Client có một vài công việc cần làm, do vậy nó định nghĩa những công việc đó trong lớp trừu tượng ClientInterface. Các lớp con của ClientInterface có thể định nghĩa theo bất kỳ cách nào mà chúng muốn. Do vậy, hành vi của Client có thể mở rộng và thay đổi bằng cách tạo ra lớp con của ClientInterface.

Bạn có thể đặt câu hỏi, tại sao tôi đặt tên là ClientInterface như trên. Tại sao tôi không gọi là AbstractInterface? Lý do, sẽ như những gì chúng ta thấy ở sau, *class trừu tượng sẽ gần gũi với class sử dụng nó hơn là class thực hiện nó.*

Hình 9-3 mô tả một kiến trúc khác. Class Policy có một tập các phương thức công khai nhằm thực hiện chính sách của một vài đối tượng. Giống như với các phương thức của Client ở hình 9-2. Cũng như trên, những hàm thực hiện chính sách đó định nghĩa một số công việc cần được thực hiện trong phạm trù của một lớp trừu tượng. Tuy nhiên, trong trường hợp này, lớp trừu tượng là một phần của chính lớp Policy. Trong C++, nó cần là một hàm ảo thuần túy (pure virtual function) và trong Java chúng cần là phương thức trừu tượng (abstract method). Những hàm này được định nghĩa trong các lớp kế cận của Policy. Do vậy, các hành vi được đưa ra trong class Policy có thể mở rộng và thay đổi trong các lớp kế cận được sinh ra từ Policy.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-9-3.png)

Hai mô hình trên là hai cách phổ biến nhất để thỏa mãn OCP. Nó cho thấy sự tách biệt rõ ràng giữa các hàm tổng quát với các hàm chi tiết được tạo ra.

**Ứng dụng Shape**

Ví dụ sau được nêu ra trong rất nhiều sách về thiết kế hướng đối tượng (OOD). Nó là một ví dụ sai lầm nổi tiếng "Shape". Nó thường được dùng để miêu tả cách mà đa hình hoạt động. Tuy nhiên, chúng ta sẽ dùng nó ở đây để làm rõ OCP.

Chúng ta có một ứng dụng có thể vẽ hình tròn và hình vuông trên giao diện đồ họa cơ bản. Hình tròn và hình vuông cần được vẽ theo một thứ tự nhất định. Một danh sách những hình tròn và hình vuông sẽ được tạo ra theo một thứ tự thích hợp, và chương trình cần kiểm tra lần lượt danh sách đó và vẽ các hình thích hợp.

**Vi phạm OCP**

Trong C, sử dụng lập trình thủ tục sẽ không thỏa mãn OCP. Chúng ta có thể giải bài toán như đoạn code dưới đây. Ở đây chúng ta định nghĩa một tập các cấu trúc dữ liệu có khởi tạo như nhau nhưng sẽ khác nhau khi chạy chương trình. Thành phần đầu tiên của mỗi cấu trúc sẽ là một mã để phân biệt xem đó là hình vuông hay hình chữ nhật. Hàm DrawAllShapes duyệt một mảng các con trỏ đến cấu trúc dữ liệu này, kiểm tra mã hình sau đó gọi hàm thích hợp (DrawCircle hay DrawSquare).

```C
// -- shape.h --
enum ShapeType {circle, square};

struct Shape
{
  ShapeType itsType;
}

// -- circle.h --
struct Circle
{
  ShapeType itsType;
  double itsRadius;
  Point itsCenter;
}

void DrawCircle(struct Circle*);

// -- square.h --
struct Square
{
  ShapeType itsType;
  double itsSide;
  Point itsTopLeft;
}

void DrawSquare(struct Square*);

// -- drawAllShapes.c --
typedef struct Shape *ShapePointer;

void DrawAllShapes(ShapePointer list[], int n)
{
  int i;
  for (i = 0; i < n; i++)
  {
    struct Shape* s = list[i];
	switch (s->itsType)
	{
	case square:
	  DrawSquare((Struct Square*) s);
	  break;
	case circle:
	  DrawCircle((Struct Circle*) s);
	  break;
	}
  }
}
```

Hàm DrawAllShapes không thỏa mãn OCP bởi nó không thể đóng với một loại hình mới. Nếu tôi muốn mở rộng hàm này để có thể vẽ một danh sách hình trong đó có cả hình tam giác, tôi sẽ phải thay đổi chính hàm này. Thực sự, tôi cần phải thay đổi hàm này để nó có thể chấp nhận bất cứ hình gì.

Đương nhiên chương trình trên là một chương trình rất đơn giản. Trong thực tế, câu lệnh switch trong hàm DrawAllShapes sẽ phải lặp đi lặp lại rất nhiều lần ở các hàm trong toàn ứng dụng, mỗi hàm sẽ có một công việc hơi khác nhau một chút. Đó có thể là hàm để kéo thả hình, mở rộng hình, di chuyển hình, xóa,... Việc thêm một hình cho ứng dụng loại này có nghĩa là sẽ phải săn tìm tất cả những nơi mới có câu lệnh switch, (hoặc có thể là if/else) và sau đó là thêm với hình mới.

Hơn nữa, chúng ta không chắc là các câu lệnh switch hay if/else sẽ được tổ chức giống như trong hàm DrawAllShapes. Trong thực tế, điều thường diễn ra là chúng ta sẽ kết hợp câu lệnh điều kiện của if với các toán tử logic, hoặc ghép các trường hợp của câu lệnh switch lại để "đơn giản hóa" phép toán logic của chúng ta. Trong một vài trường hợp, sẽ có những hàm làm việc với hình vuông giống hệt với hình tròn. Những hàm đó còn chả có câu lệnh switch hay if/else. Do vậy, vấn đề của việc tìm kiếm và hiểu tất cả những nơi mà hình mới cần thêm vào trở nên không đơn giản chút nào.

Tiếp theo, hành xem xét loại thay đổi mà chúng ta sẽ làm. Chúng ta cần thêm một thành viên cho kiểu dữ liệu enum ShapeType. Bởi tất cả hình sẽ phục thuộc vào kiểu dự liệu này, chúng ta sẽ phải biên dịch lại chúng từ đầu. Chúng ta cũng phải biên dịch lại tất cả các module có liên quan đến Shape.

Do vậy, chúng ta không những chỉ phải thay đổi câu lệnh switch hay if/else, mà còn phải thay đổi các tập tin nhị phân (thông qua quá trình biên dịch lại) của tất cả các module sử dụng cấu trúc dữ liệu Shape. Thay đổi tập tin nhị phân có nghĩa là thay đổi DLL, thư viện chia sẻ hay bất cứ thành phần nhị phân nào có liên quan. Hành động đơn giản là thêm một hình kéo theo hàng loạt thay đổi đến hệ thống từ mã nguồn cho đến các tập tin nhị phân. Rõ ràng, ảnh hưởng của việc thêm một hình là rất lớn.

**Một design tệ**

Cùng nhìn lại một lần nữa. Lời giải đưa ra ở trên là cứng nhắc (Rigid) bởi việc thêm hình tam giác làm cho Shape, Square, Circle, DrawAllShapes phải được biên dịch lại. Nó dễ vỡ (Fragile) bởi rất nhiều câu lệnh switch/case hoặc if/else khó để tìm kiếm cũng như sửa chữa đúng đắn. Nó bất động (Immobile) bởi bất cứ ai muốn tái sử dụng DrawAllShapes trong chương trình khác sẽ phải mang theo cả Square và Circle kể cả khi họ không cần nó. Do vậy, đoạn code trên đã có quá nhiều dấu hiệu của thiết kế tồi.

**Một thiết kế thỏa mãn OCP**

Đoạn code sau đây cung cấp lời giải cho bài toán hình tròn\hình vuông thỏa mãn OCP. Trong trường hợp này, chúng ta tạo ra một lớp trừu tượng `Shape`. Lớp trừu tượng này có một hàm trừu tượng là Draw. Cả hai class `Circle` và `Square` đều được sinh ra từ class `Shape`.

```C#
// Lời giải OOD cho bài toán Square/Circle
class Shape
{
  public:
    virtual void Draw() const = 0;
};

class Square : public Shape
{
  public:
    virtual void Draw() const;
};

class Circle : public Shape
{
  public:
    virtual void Draw() const;
};

void DrawAllShapes(vector<Shape*>& list)
{
  vector<Shape*>::iterator i;
  for (i=list.begin(); i != list.end(); i++) {
    (*i)->Draw();
  }
}
```
Chú ý rằng nếu chúng ta muốn mở rộng hành vi của hàm `DrawAllShapes` để vẽ được hình khác, tất cả những gì chúng ta cần là tạo ra một class sinh từ `Shape`. Hàm `DrawAllShapes` không cần phải thay đổi. Do vậy hàm `DrawAllShapes` thỏa mãn OCP. Hành vi của nó có thể mở rộng mà không cần thay đổi nó. Thật sự, việc thêm class `Triangle` tuyệt đối không tạo ra bất cứ ảnh hưởng nào đến bất cứ module nào đã tạo ra. Rõ ràng một vài thành phần của hệ thống cần thay đổi để làm việc với class `Triangle`, nhưng tất cả những dòng code ở trên đều không cần thay đổi.

Trong thực tế, class `Shape` có thể có nhiều phương thức hơn. Tuy nhiên, việc thêm hình vào trong ứng dụng của chúng ta là rất đơn giản bởi tất cả những gì chúng ta cần là tạo thêm class thực hiện `Shape`. Chúng ta không cần phải dò tìm toàn ứng dụng xem chỗ nào cần thay đổi. Do vậy, đây là một lời giải không dễ vỡ (not Fragile).

Nó cũng không cứng nhắc (not Rigid). Không có mã nguồn nào cần thay đổi, và nếu không có ngoại lệ, thì cũng không có tập tin nhị phân nào bị thay đổi. module tạo ra thực thể cho class mới thực hiện Shape cần thay đổi. Thông thường là ở main hoặc hàm mà main gọi tới, hoặc phương thức của đối tượng tạo ra bởi main.

Cuối cùng, đây là một lời giải không bất động (not Immobile). `DrawAllShapes` có thể tái sử dụng ở bất cứ ứng dụng nào mà không cần phải mang theo `Square` hay `Circle`. Do vậy, lời giải này không có dấu hiệu của bất cứ đặc tính nào của thiết kế tồi.

Lời giải này thỏa mãn OCP. Nó có thể thay đổi bằng cách thêm code mới, chứ không phải sửa code có sẵn. Do vậy, nó không gây ra các thay đổi ở những chương trình liên quan. Sự thay đổi duy nhất là việc thêm module và thay đổi liên quan đến main cho phép thực thể mới được tạo ra.

**OK, Tôi đã nói dối**

Ví dụ trên là một ví dụ hoàn toàn có trật tự. Hãy xem xét điều gì xảy ra với `DrawAllShapes` nếu chúng ta quyết định tất cả các hình tròn cần được vẽ trước hình vuông. Hàm `DrawAllShapes` không thể đóng với những thay đổi như vậy. Để thực hiện thay đổi đó, bạn cần phải vào trong hàm `DrawAllShapes`, kiểm tra tất cả hình tròn, rồi mới đến hình vuông.

