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

```C++
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

**Dự đoán và kiến trúc "tự nhiên"**

Liệu chúng ta đã dự đoán được kiểu thay đổi này và đưa ra một lớp trừu tượng bảo vệ chúng ta khỏi nó. Cách trừu tượng hóa đưa ra ở đoạn code trên chỉ làm chương ngại cho chúng ta với loại thay đổi này, hơn là một sự giúp đỡ. Bạn có thể bất ngờ. Nhưng cuối cùng, điều gì có thể tự nhiên hơn một lớp Shape làm cơ sở để Square và Circle sinh ra? Tại sao mô hình tự nhiên này lại không phải mô hình tốt nhất? Đáp án rõ ràng là mô hình sẽ KHÔNG tự nhiên trong hệ thống mà thứ tự quan trọng hơn kiểu hình dạng.

Nó đưa chúng ta đến một kết luận phiền phức. Một cách tổng quá, không cần biết module của bạn "đóng" như thế nào, sẽ luôn có một loại thay đổi biến nó thành không đóng. Không có mô hình nào tự nhiên trong mọi hoàn cảnh!

Bởi tính đóng không thể hoàn thiện được, nó cần được xử lý một cách chiến lược. Có nghĩa là, người thiết kế cần lựa chọn loại thay đổi so với cái có thể đóng thiết kế của họ. Anh ta cần dự đoàn hầu hết các thay đổi có thể xảy ra, và sau đó xây dựng kiến trúc trừu tượng để bảo vệ mình khỏi thay đổi đó.

Điều này cần nhiều kinh nghiệm của người thiết kế. Người thiết kế có kinh nghiệm hy vọng anh ta biết được người dùng và môi trường kinh doanh đủ để có thể dự đoán các loại thay đổi đến hệ thống. Sau đó, anh ta sử dụng OCP để đáp ứng thay đổi có tần suất cao nhất.

Đây không phải là điều đơn giản. Khi lập trình viên đoán đúng, họ thắng. Khi họ đoán sai, họ thua. Và họ thường đoán sai hầu hết các trường hợp.

Thêm vào đó, đáp ứng được OCP rất tốn kém. Nó tiêu tốn nhiều thời gian phát triển và nỗ lực để tạo ra một kiến trúc hợp lý. Tính trừu tượng cũng làm tăng lên độ phức tạp của ứng dụng. Có một lượng giới hạn sự trừu tượng mà lập trình viên có thể đáp ứng được. Rõ ràng, chúng ta cần giới hạn OCP cho một những thay đổi hay xảy ra nhất.

Làm cách nào chúng ta biết được thay đổi nào dễ xảy ra? Chúng ta cần nghiên cứu hợp lý, hỏi câu hỏi hợp lý, và dùng kinh nghiệm của mình cũng như những kiến thức chung. Và cuối cùng, *chúng ta đợi đến khi thay đổi diễn ra!*

**Cho "lưỡi câu" vào trong**

Làm cách nào chúng ta bảo vệ mình trước những thay đổi? Ở thế kỷ trước, chúng tôi có một câu nói. Chúng ta nên "cho lười câu vào trong" cho thay đổi mà chúng ta nghĩ là nó có thể xảy ra. Chúng tôi nghĩ rằng điều đó có thể giúp phần mềm trở nên mềm dẻo.

Tuy nhiên, lưỡi câu chúng tôi cho vào thường sai lầm. Sai lầm hơn nữa, nó lại có dấu hiệu của sự phức tạp không cần thiết (Needless Complexity) cần được hộ trợ vào bảo trì, kể cả khi chúng không được dùng đến. Đó không phải là một điều tốt. Chúng ta không muốn sử dụng một thiết kế có quá nhiều lớp trừu tượng. Thay vào đó, chúng ta thường đợi cho đến khi chúng ta thực sự cần trừu tượng hóa, rồi mới phát triển nó.

**Lừa tôi một lần...**

Có một câu nói từ xa xưa: "Lừa tôi một lần, bạn xấu hổ. Lừa gạt tôi hai lần, tôi xấu hổ." Đây là một thái độ mạnh mẽ trong thiết kế phần mềm. Để ứng dụng của chúng ta thoát khỏi sự phức tạp không cần thiết (Needless Complexity), chúng ta có thể cho phép bản thân lừa mình một lần. Có nghĩa là chúng ta viết code và hy vọng sẽ không có thay đổi. Khi thay đổi xảy ra, chúng ta xây dựng lớp trừu tượng để bảo vệ mình khỏi những thay đổi trong tương lai có dạng đó. Nói ngắn gọn, chúng ta tóm viên đạn đầu tiên, và chắc chắn rằng mình sẽ được bảo vệ từ bất cứ viên đạn nào bắn ra từ cùng khẩu súng đó.

**Mô phỏng thay đổi**

Nếu chúng ta quyết định tóm viên đạn đầu tiên, chúng ta có lợi thế biết được hướng bay của viên đạn sớm hơn. Chúng ta biết được loại thay đổi có thể diễn ra trước khi chúng ta đi quá xa trong lộ trình phát triển. Chúng ta càng đợi lâu để thấy được thay đổi đó, chúng ta càng khó để tạo ra một lớp trừu tượng hợp lý.

Do vậy, chúng ta cần mô phỏng thay đổi. Chúng ta làm điều đó thông qua hàng loạt cuộc thỏa luận đưa ra ở chương 2.

* Chúng ta viết kịch bản kiểm thử trước. Kiểm thử là một loại hình sử dụng hệ thống. Bằng cách viết kiểm thử trước, chúng ta buộc hệ thống trở nên có thể kiểm thử được (testable). Do vậy, thay đổi trong một hệ thống có thể kiểm thử sẽ không làm chúng ta quá bất ngờ sau đó. Chúng ta sẽ phải xây dựng lớp trừu tượng có thể kiểm thử được. Chúng ta sẽ thấy rằng rất nhiều trong số các lớp trừu tượng đó giúp ích chúng ta khi các thay đổi sau này diễn ra.

* Chúng ta phát triển trên càng vòng đời ngắn. Vài ngày thay vì vài tuần.

* Chúng ta phát triển các chức năng trước cơ sở hạ tầng, và thường xuyên cho các bên liên quan thấy các chức năng đã làm được.

* Chúng ta phát triển các chức năng quan trọng nhất trước.

* Chúng ta phân phối phần mềm nhanh và thường xuyên. Chúng ta đưa nó đến với khách hàng và người dùng nhanh chóng và thường xuyên nhất có thể.

**Sử dụng sự trừu tượng để đạt được tính đóng một cách minh bạch**

OK, vậy chúng ta đã tóm được viên đạn đầu tiên. Người dùng muốn vẽ hình tròn trước hình vuông. Giờ chúng ta phải bảo vệ bản thân khỏi những thay đổi có dạng này trong tương lai.

Làm cách nào để chúng ta đóng được hàm `DrawAllShapes` trước các thay đổi về thứ tự vẽ? Nhớ rằng tính đóng phụ thuộc vào sự trừu tượng. Do vậy, để đóng được hàm `DrawAllShapes` trước sự thay đổi, chúng ta cần sự trừu tượng về thứ tự. Loại trừu tượng này cung cấp một lớp có thể mô tả được thứ tự vẽ.

Mỗi quy tắc sắp xếp sẽ gồm hai đối tượng, để có thể xác định đối tượng nào được vẽ trước. Chúng ta có thể định nghĩa phương thức trừu tượng cho `Shape` tên là `Precedes`. Hàm này nhận một hình khác làm tham số và trả về giá trị luận lý (boolean). Kết quả là đúng (true) nếu đối tượng Shape nhận được thông báo cần được vẽ trước khi đối tượng `Shape` được truyền vào dưới dạng tham số.

Trong C++, phương thức có dạng này được biểu thị dưới dạng `operator<`. Đoạn code sau đây mô tả class `Shape` sẽ như thế nào khi có thêm phương thức sắp xếp.

```C++
// Shape with ordering method
class Shape
{
  public:
	virtual void Draw() const = 0;
	virtual bool Precedes(const Shape&) const = 0;

	bool operator<(const Shape& s) {return Precedes(s);}
}
```
Đến đây, chúng ta có thể xác định quan hệ sắp xếp giữa hai đối tượng `Shape`. Chúng ta có thể sắp xếp chúng rồi mới vẽ. Đoạn code sau viết bằng C++ sẽ thực hiện công việc đó.

```C++
// DrawAllShapes with Ordering
template <typename P>
class Lessp // utility for sorting container or pointers
{
  public:
    bool operator()(const P p, const P q) {return (*p) < (*q);}
}

void DrawAllShapes(vector<Shape*>& list)
{
  vector<Shape*> orderedList = list;
  sort(orderedList.begin(),
       orderedList.end(),
	   Lessp<Shape*>*());

  vector<Shape*>::const_iterator i;
  for (i=orderedList.begin(); i < orderedList.end(); i++)
	(*i)->Draw();
}
```
Bây giờ chúng ta có thể vẽ hình và vẽ theo thứ tự định sẵn. Tuy nhiên có vẻ chúng ta chưa có giải pháp cho việc vẽ theo thứ tự giảm dần mà bài toán yêu cầu. Do vậy, chúng ta cần sửa hàm `Precedes` của class `Shape` trong các lớp con để chỉ định thứ tự sắp xếp. Làm cách nào để thự hiện điều đó? Dòng code nào nên ghi ở `Circle::Precedes` để đảm bảo hình tròn sẽ được vẽ trước hình vuông? Xem xét đoạn code sau.

```C++
// Ordering a Circle
bool Circle::Precedes(const Shape& s) const
{
  if (dynamic_cast<Square*>(s))
    return true;
  else
    return false;
}
```
Rõ ràng ở phương thức trên, cũng như những người anh em của nó cùng sinh ra từ Shape, không thỏa mãn OCP. Không có cách nào để đóng chúng trước những lớp con mới của Shape. Mỗi khi có một lớp con được sinh ra, tất cả các hàm Precedes cần thay đổi.

Tất nhiên sẽ chẳng có vấn đề gì nếu không có thêm lớp con nào sinh ra từ Shape. Ngược lại, nếu chúng được tạo ra thường xuyên, thiết kế sẽ phải thay đổi rất nhiều. Vậy, một lần nữa, chúng ta lại bắt được viên đạn.

**Sử dụng cách tiếp cận định hướng từ dữ liệu để đạt được tính đóng**

Nếu chúng ta muốn đóng các con của Shape để chúng không cần biết các con khác, chúng ta có thể tiếp cận từ dữ liệu. Dưới đây là một giải pháp.

```C++
// Cơ chế quản lý sắp xếp sử dụng bảng
#include <typeinfo>
#include <string>
#include <iostream>

using namespace std;

class Shape
{
  public:
    virtual void Draw() const = 0;
	bool Precedes(const Shape&) const;
	bool operator<(const Shape& s) const {return Precedes(s);}
	static void const char* typeOrderTable[];
};

const char * Shape::typeOrderTable[] =
{
  typeid(Circle).name(),
  typeid(Square).name(),

};

// Hàm sau tìm kiếm bảng theo tên class.
// Bảng định nghĩa thứ tự mà các hình được vẽ.
// Hình không tìm được luôn được sắp xếp trước hình tìm được.
bool Shape::Precedes(const Shape& s) const
{
  const char* thisType = typeid(*this).name();
  const char* argType = typeid(s).name();
  bool done = false;
  int thisOrd = -1;
  int argOrd = -1;
  for (int i = 0; !done; i++) {
    const char* tableEntry = typeOrderTable[i];
	if (tableEntry != 0)
	{
	  if (strcmp(tableEntry, thisType) == 0)
	    thisOrd = i;
	  if (strcmp(tableEntry, argType) == 0)
	    argOrd = i;
	  if ((argOrd >= 0) && (thisOrd >= 0))
	    done = true;
	}
	else // table entry == 0
	  done = true;
  }

  return thisOrd < argOrd;
}
```
Bằng cách tiếp cận này, chúng ta đã thành công trong việc đóng hàm `DrawAllShapes` trước những thay đổi về thứ tự các hình, trong đó có việc thêm hình mới hoặc thay đổi thứ tự sẵn có (ví dụ vẽ hình vuông trước hình tròn).

Có duy nhất một thành phần không đóng trước những thay đổi về thứ tự là bảng của `Shape`. Bảng này có thể được đặt ở một module riêng của Shape, tách biệt với các module còn lại cho nên thay đổi ở đó không ảnh hưởng đến module khác. Thực tế, với C++, chúng ta có thể lựa chọn bảng nào sẽ sử dụng trong khi thiết lập liên kết.

**Tóm lại**

Theo nhiều cách, OCP là trái tim của thiết kế hướng đối tượng. Thỏa mãn nguyên tắc này là điều tạo nên lợi ích lớn nhất từ kỹ thuật lập trình hướng đối tượng (tính mềm dẻo, tính tái sử dụng được, tính bảo trì,...). Tuy nhiên, thỏa mãn được nguyên lý này không đạt được một cách đơn giản bởi sử dụng ngôn ngữ lập trình hướng đối tượng. Cũng như không nên áp dụng hàng tá lớp trừu tượng cho tất cả các thành phần của ứng dụng. Thay vào đó, nó yêu cầu lập trình viên sử dụng tính trừu tượng cho những phần mà thường xuyên có thay đổi một cách rõ ràng. *Chống lại sự trừu tượng ở dạng non nớt cũng quan trọng như bản thân sự trừu tượng.*


# 3. DIP: The Dependency-Inversion Principle

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/DIP-Image.png)

  * Các module cấp cao không nên phụ thuộc vào các modules cấp thấp. Cả 2 nên phụ thuộc vào abstraction. (High-level modules should not depend on low-level modules. Both should depend on abstractions.)
  * Interface (abstraction) không nên phụ thuộc vào chi tiết, mà chi tiết nên phụ thuộc vào abstraction. (Abstractions should not depend on details. Details should depend on abstractions.)

Trong những năm qua, nhiều người đã đặt câu hỏi tại sao tôi sử dụng từ "inversion" trong tên của nguyên tắc này. Phương pháp phát triển phần mềm truyền thống hơn chẳng hạn như Phân tích và Thiết kế cấu trúc có xu hướng tạo cấu trúc phần mềm trong đó các module cấp cao phụ thuộc vào module cấp thấp.
Cấu trúc phụ thuộc của một chương trình hướng đối tượng, được thiết kế tốt là "đảo ngược" so với cấu trúc phụ thuộc so với phương pháp truyền thống.

Xem xét việ tác động của các module cấp cao phụ thuộc vào module cấp thấp. Đó là các module cấp cao chứa các quyết định chính sách và mô hình hoạt động quan trọng của một ứng dụng. Mang tính chất quyết định quan trọng trong ứng dụng. Tuy nhiên, khi các module này phụ thuộc vào các module cấp thấp hơn, các thay đổi đối với các module cấp thấp hơn có thể có ảnh hưởng trực tiếp đến các module cấp cao hơn và có thể bắt buộc phải thay đổi chúng.

Tình trạng khó khăn này là vô lý! Đó là các module thiết lập chính sách, cấp cao, lẽ ra phải ảnh hưởng đến các module chi tiết, cấp thấp. Các module chứa các quy tắc nghiệp vụ cấp cao sẽ được ưu tiên hơn và độc lập với các module có chứa các chi tiết triển khai. Các module cấp cao đơn giản là không nên
phụ thuộc vào các module cấp thấp theo bất kỳ cách nào.

Hơn nữa, đó là các module thiết lập chính sách, cấp cao mà chúng tôi muốn có thể sử dụng lại. Chúng ta đã làm khá tốt trong việc sử dụng lại các module cấp thấp dưới dạng thư viện chương trình con. Khi các module cấp cao phụ thuộc vào các module cấp thấp, sẽ rất khó sử dụng lại các module cấp cao đó trong các ngữ cảnh khác nhau. Tuy nhiên, khi các module cấp cao độc lập với các module cấp thấp, thì các module cấp cao có thể được sử dụng lại khá đơn giản. Nguyên tắc này là trọng tâm của thiết kế framework.

**Phân lớp**

Theo Booch, “... tất cả các kiến trúc hướng đối tượng có cấu trúc tốt đều có các lớp được xác định rõ ràng, với mỗi lớp cung cấp một số dịch vụ nhất quán thông qua một giao diện được xác định và kiểm soát tốt.” Một cách diễn giải ngây thơ về tuyên bố này có thể khiến nhà thiết kế tạo ra một cấu trúc tương tự như Hình 11-1. Một cách hiểu đơn giản trong sơ đồ này, lớp Policy cấp cao sử dụng lớp Mechanism cấp thấp hơn, lớp này lại sử dụng lớp Utility cấp chi tiết. Mặc dù điều này có vẻ phù hợp, nó có đặc điểm khó hiểu mà lớp Policy nhạy cảm với thay đổi hoàn toàn trong lớp Utility. *Sự phụ thuộc có tính bắt cầu*. Lớp Policy phụ thuộc vào một thứ phụ thuộc vào lớp Utility; do đó, lớp Policy chuyển tiếp phụ thuộc vào lớp Uitility.
Điều này là rất đáng tiếc.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-11-1.png)

Hình 11-2 cho thấy một mô hình thích hợp hơn. Mỗi lớp cấp cao khai báo một giao diện trừu tượng cho các dịch vụ mà nó cần. Các lớp cấp thấp hơn sau đó được hiện thực hóa từ các giao diện trừu tượng này. Mỗi lớp cấp cao hơn sử dụng lớp thấp nhất tiếp theo thông qua giao diện trừu tượng. Như vậy, các lớp cấp cao không phụ thuộc vào các lớp cấp thấp. Thay vào đó, các lớp cấp thấp phụ thuộc vào các giao diện dịch vụ trừu tượng được khai báo ở các lớp cấp cao. 

**Sự đảo ngược quyền sở hữu**

Lưu ý rằng sự đảo ngược ở đây không chỉ là một trong những phần phụ thuộc. Nó cũng là một trong những quyền sở hữu giao diện. Nhưng khi DIP được áp dụng, chúng ta thấy rằng các máy khách có xu hướng sở hữu các abstract interfaces và các máy chủ của họ bắt nguồn từ chúng.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-11-2.png)

Điều này đôi khi được gọi là nguyên tắc của Hollywood: "Đừng gọi cho chúng tôi, chúng tôi sẽ gọi cho bạn." Các module cấp thấp thực hiện việc implemmentation interfaces và được gọi bởi các module cấp cao hơn. 

Sử dụng quyền sở hữu "đảo ngược" này, `PolicyLayer` không bị ảnh hưởng bởi bất kỳ thay đổi nào đến `MechanismLayer` hoặc `UtilityLayer`. Hơn thế nữa, có thể sử dụng lại trong bất kỳ ngữ cảnh nào xác định các mô-đun cấp thấp hơn tuân theo `PolicyServiceInterface`. Do đó, bằng cách đảo ngược các phần phụ thuộc. Do đó, bằng cách đảo ngược các yếu tố phụ thuộc, chúng tôi đã tạo ra một cấu trúc, đồng thời linh hoạt hơn, bền hơn và cơ động hơn

**Phụ thuộc vào Abstractions**

Với cách nói khác nhưng cũng khá là đúng về DIP "Phụ thuộc vào abstractions". Nói một cách đơn giản là không nên phụ thuộc vào một class cụ thể, các mối quan hệ trong một chương trình nên kết thúc trên một class abtracts hoặc một interface.

  * Không có biến nào nên giữ một con trỏ hoặc tham chiếu đến một lớp cụ thể.
  * Không có lớp nào nên bắt nguồn từ một lớp cụ thể.
  * Không có phương thức nào nên ghi đè một phương thức đã triển khai của bất kỳ lớp cơ sở nào của nó.

Chắc chắn những điều này thường bị vi phạm ít nhất một lần trong mọi chương trình. Ai đó phải tạo các phiên bản của các lớp cụ thể và bất kỳ module nào làm được điều đó sẽ phụ thuộc vào chúng.

**Một ví dụ đơn giản**

Đảo ngược sự phụ thuộc có thể được áp dụng ở bất cứ nơi nào một lớp gửi một message đến lớp khác. Ví dụ, hãy xem xét trường hợp của đối tượng `Button` và đối tượng `Lamp`.

Đối tượng `Button` cảm nhận những thay đổi môi trường bên ngoài. Khi nhận được thông báo từ `Poll` , nó sẽ xác định xem người dùng có "nhấn" nó hay không. Không quan trọng cơ chế cảm biến là gì. Đó có thể là biểu tượng nút trên GUI, nút vật lý được nhấn bằng ngón tay người hoặc thậm chí là thiết bị phát hiện chuyển động trong hệ thống an ninh gia đình. Đối tượng `Button` phát hiện người dùng đã kích hoạt hoặc hủy kích hoạt nó.

Đối tượng Đèn ảnh hưởng đến môi trường bên ngoài. Khi nhận được tin nhắn TurnOn, nó sẽ sáng lên một thứ ánh sáng nào đó. Khi nhận được thông báo TurnOff, nó sẽ tắt ánh sáng đó. Cơ chế vật lý không quan trọng. Đó có thể là đèn LED trên bảng điều khiển máy tính, đèn hơi thủy ngân trong bãi đậu xe, hoặc thậm chí là tia laser trong máy in laser.
Làm thế nào chúng ta có thể thiết kế một hệ thống sao cho đối tượng Nút điều khiển đối tượng `Lamp`? Hình 11-3 cho thấy một naive. Đối tượng `Button` nhận thông báo `Poll`, xác định xem nút đã được nhấn hay chưa, sau đó chỉ cần gửi thông báo TurnOn hoặc TurnOff tới `Lamp`.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-11-3.png)

Tại sao điều này lại ngây thơ? Hãy xem xét đoạn code Java đang mô tả mô hình này (Listing 11-1). Lưu ý rằng lớp `Button` phụ thuộc trực tiếp vào lớp `Lamp`. Sự phụ thuộc này ngụ ý rằng `Button` sẽ bị ảnh hưởng bởi các thay đổi đối với `Lamp`. Hơn nữa, sẽ không thể sử dụng lại `Button` để điều khiển một đối tượng `Motor`. Trong thiết kế này, các đối tượng `Button` điều khiển các đối tượng `Lamp` và chỉ các đối tượng `Lamp`.

Listing 11-1
Button.java
```Java
    public class Button
    {
      private Lamp itsLamp;
      public void poll()
      {
        if (/*some condition*/)
         itsLamp.turnOn();
      }
    }
```

Giải pháp này vi phạm DIP. 

**Tìm kiếm sự trừu tượng cơ bản**

Chính sách cấp cao là gì? đó là abstraction nền của ứng dụng, luôn đảm bảo được sự đúng đắn khi các chi tiết thay đổi. Nó là hệ thống bên trong hệ thống. Trong `Button/Lamp` là một ví dụ, phần trừu tượng cơ bản là phát hiện cử chỉ bật / tắt từ người dùng và chuyển tiếp cử chỉ đó đến đối tượng mục tiêu. Cơ chế nào được sử dụng để phát hiện cử chỉ của người dùng?

Thiết kế trong Hình 11-3 có thể được cải thiện bằng cách đảo ngược sự phụ thuộc vào đối tượng `Lamp`. Trong Hình 11-4, chúng ta thấy rằng `Button` bây giờ giữ một liên kết với một thứ gọi là `ButtonServer`. `ButtonServer` cung cấp các phương thức trừu tượng mà `Button` có thể sử dụng để bật hoặc tắt một cái gì đó. `Lamp` thực hiện implements `ButtonServer` interface. Bây giowfw, `Lamp` thực hiện sự phụ thuộc thay vì bị phụ thuộc.

Thiết kế trong Hình 11-4 cho phép một `Button` điều khiển bất kỳ thiết bị nào sẵn sàng triển khai interface ButtonServer. Điều này mang lại cho chúng tôi rất nhiều tính linh hoạt. Nó cũng có nghĩa là các đối tượng `Button` sẽ có thể điều khiển các đối tượng chưa invented.

Tuy nhiên, giải pháp này cũng đặt ra một hạn chế đối với bất kỳ đối tượng nào cần được điều khiển bằng `Button`. Một đối tượng như vậy phải triển khai giao diện ButtonServer. Điều này thật đáng tiếc vì những đối tượng này cũng có thể muốn được điều khiển bởi một đối tượng `Switch` hoặc một số đối tượng không phải là `Button`.

Bằng cách đảo ngược hướng của sự phụ thuộc và làm cho `Lamp` làm việc phụ thuộc thay vì bị phụ thuộc, chúng tôi đã làm cho `Lamp` phụ thuộc vào một chi tiết khác — `Button`. 

`Lamp` chắc chắn phụ thuộc vào `ButtonServer`, nhưng `ButtonServer` không phụ thuôc vào `Button`. Bất kỳ loại đối tượng nào biết cách vận dụng `ButtonServer` interface sẽ có thể điều khiển `Button`.
Trong trường hợp này, không ai sở hữu interface. Chúng tôi có một tình huống thú vị là interface có thể được sử dụng bởi nhiều clients khác nhau và được thực hiện bởi nhiều servers khác nhau.

**Ví dụ về lò nung**

Hãy xem một ví dụ thú vị hơn. Xem xét phần mềm có thể kiểm soát bộ điều chỉnh của lò. Phần mềm có thể đọc nhiệt độ hiện tại từ một kênh IO và hướng dẫn bật hoặc tắt lò bằng cách gửi lệnh đến một kênh IO khác. Cấu trúc của thuật toán có thể trông giống như Listing 11-2.
Listing 11-2
Simple algorithm for a thermostat

```C++
#define TERMOMETER 0x86
#define FURNACE 0x87
#define ENGAGE 1
#define DISENGAGE 0
void Regulate(double minTemp, double maxTemp)
{
 for(;;)
 {
 while (in(THERMOMETER) > minTemp)
 wait(1);
 out(FURNACE,ENGAGE);
 
 while (in(THERMOMETER) < maxTemp)
 wait(1);
 out(FURNACE,DISENGAGE);
 }
}
```

Mục đích của thuật toán là rõ ràng, nhưng mã lộn xộn với nhiều chi tiết cấp thấp. Mã này không bao giờ có thể được sử dụng lại với các phần cứng điều khiển khác nhau.
Điều này có thể không gây mất mát nhiều vì source code là nhỏ. Nhưng ngay cả như vậy, thật đáng tiếc khi có thuật toán không thể sử dụng lại. Chúng tôi muốn đảo ngược các phần phụ thuộc và xem một cái gì đó giống như Hình 11-5.

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-11-4.png)

Điều này cho thấy rằng hàm điều chỉnh nhận hai đối số là cả hai interfaces. Giao diện `Thermometer` có thể được đọc, và giao diện `Heater` có thể được kết nối và ngắt kết nối. Đây là tất cả các thuật toán `Regulate` nhu cầu. Bây giờ nó có thể được viết như thể hiện trong Listing 11-3.

Listing 11-3
Generic Regulator
```C++
void Regulate(Thermometer& t, Heater& h,
 double minTemp, double maxTemp)
{
 for(;;)
 {
 while (t.Read() > minTemp)
 wait(1);
 h.Engage();
 while (t.Read() < maxTemp)
 wait(1);
 h.Disengage();
 }
}
```
Điều này đã đảo ngược sự phụ thuộc để chính sách quy định cấp cao không phụ thuộc vào bất kỳ chi tiết cụ thể nào của nhiệt kế hoặc lò nung. Thuật toán có thể tái sử dụng.

**Đa hình động v. Tĩnh**

Chúng tôi đã đạt được sự đảo ngược của các phần phụ thuộc và làm cho Quy định chung chung, thông qua việc sử dụng động
đa hình (abstract class hoặc interfaces). Tuy nhiên, có một cách khác. Chúng tôi có thể đã sử dụng tĩnh dạng đa hình được tạo bởi các mẫu C ++. Xem xét Liệt kê 11-4.

Listing 11-4

```C++
template <typename THERMOMETER, typename HEATER>
class Regulate(THERMOMETER& t, HEATER& h,
 double minTemp, double maxTemp)
{
 for(;;)
 {
 while (t.Read() > minTemp)
 wait(1);
 h.Engage();
 while (t.Read() < maxTemp)
 wait(1);
 h.Disengage();
 }
}
```

**Kết luận**

Lập trình thủ tục truyền thống tạo ra một cấu trúc phụ thuộc trong đó chính sách phụ thuộc vào chi tiết. Điều này thật đáng tiếc vì các chính sách sau đó dễ bị thay đổi chi tiết. Lập trình hướng đối tượng đảo ngược cấu trúc phụ thuộc sao cho cả chi tiết và chính sách đều phụ thuộc vào tính trừu tượng và các service interface. Nguyên tắc đảo ngược phụ thuộc là fundamental low-level cấu tao đằng sau nhiều lợi ích mà công nghệ hướng đối tượng mang lại. Ứng dụng thích hợp của nó là cần thiết để tạo ra các khuôn khổ có thể tái sử dụng. Nó cũng cực kỳ quan trọng đối với việc xây dựng mã có khả năng thay đổi. Vì tất cả các phần trừu tượng và chi tiết đều được tách biệt với nhau nên mã dễ bảo trì hơn nhiều.

# 4. LSP: The Liskov Substitution Principle - Nguyên tắc thay thế Liskov

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/LSP-image.png)

Nguyên lý của OCP dựa trên hai thành phần chính là trừu tượng (abstraction) và đa hình (polymorphism). Trong các ngôn ngữ lập trình với kiểu dữ liệu tĩnh như C++, Java, một trong những yếu tố hỗ trợ cho trừu tượng và đa hình là thừa kế. Bằng việc sử dụng sự thừa kế, chúng ta có thể tạo ra các class với các phương thức thực hiện dựa theo khai báo phương trức trừu tượng của class cơ sở.

LSP có thể viết ngắn gọn như sau:

`Kiểu con phải có thể thay thế được cho kiểu cơ sở - Subtypes must be substituable for their base types.`

Barbara Liskov lần đầu tiên đưa ra nguyên lý này vào năm 1988. Phát biểu của bà ấy như sau:

Điều chúng ta cần là một mô hình thỏa mãn đặc tính sau đây của sự thay thế:
Với mỗi object o1 thuộc kiểu S, có một object o2 thuộc kiểu T mà với tất cả các chương trình P được định nghĩa với T, các hành vi của P không thay đổi khi o2 được thay thế bởi o1, và khi đó S là một kiểu con của T.

Tầm quan trọng của nguyên tắc này sẽ rất rõ ràng nếu bạn thấy được hậu quả khi vi phạm nó. Giả sử chúng ta có một hàm f thuộc class cơ sở B. Giả sử chúng ta có một class D được tạo ra dựa vào B mà khi chúng ta gọi hàm f với object của D theo cái cách làm với B, f hoạt động sai. Khi đó, D sẽ vi phạm LSP. Rõ ràng, D trở nên mỏng manh với sự có mặt của f.

Tác giả của f sẽ cố gắng bổ sung một vài chi tiết để có thể kiểm tra với D do đó f có thể hoạt động tốt khi gọi nó từ một object của D. Cách làm này sẽ vi phạm OCP bởi f không hoàn toàn đóng với tất các class được sinh ra từ B. Code trở nên có mùi và nó rõ ràng là kết quả của một lập trình viên thiếu kinh nghiệm (hoặc, tồi tệ hơn, một lập trình viên vội vàng) cố gắng giải quyết mâu thuẫn với LSP.

**Một ví dụ vi phạm LSP**

Việc vi phạm LSP thường là kết quả của việc sử dụng thông tin kiểu của biến trong thời gian chạy (RTTI: Run-Time Type Information). Thông thường, các cú pháp `if, if/else` được sử dụng để xác định kiểu của biến và từ đó hành vi thích hợp được lựa chọn. Xem xét ví dụ 10-1.

```C++
struct Point {double x,y;};

struct Shape {
  enum ShapeType {square, circle} itsType;
  Shape(ShapeType t) : itsType(t) {}
};

struct Circle : public Shape
{
  Circle() : Shape(circle) {};
  void Draw() const;
  Point itsCenter;
  double itsRadius;
}

struct Square : public Shape
{
  Square() : Shape(circle) {};
  void Draw() const;
  Point itsTopLeft;
  double itsSide;
}

void DrawShape(const Shape& s)
{
  if (s.itsType == Shape::square)
    static_cast<const Square&>(s).Draw();
  else if (s.itsType == Shape::circle)
    static_cast<const Circle&>(s).Draw();
}
```
Rõ ràng, hàm `DrawShape` ở trên vi phạm OCP. Nó cần biết tất cả các class con của Shape, và cần thay đổi mỗi khi một class con mới của `Shape` được sinh ra. Nhiều người có thể dễ dàng thấy được cách làm này là ngược hoàn toàn với một thiết kế tốn. Vậy một lập trình viên tốt sẽ viết hàm này như thế nào?

Cùng xem xét cách giải quyết của lập trình viên tên Joe. Sau khi học về lập trình hướng đối tượng, anh ta đi đến kết luận là đa hình quá khó hiểu cho trường hợp này. Do vậy, anh ta định nghĩa class `Shape` mà không có bất cứ hàm ảo nào. Class (hoặc cấu trúc) `Square` và `Circle` sinh ra từ Shape và sẽ có hàm `Draw()`, nhưng chúng không ghi đè lên hàm này. Vì `Circle` và `Square` không thể hoàn toàn thay thế cho `Shape`, hàm `DrawShape` sẽ xác định tham số hình cần vẽ và gọi hàm `Draw` thích hợp.

Vấn đề là, `Square` và `Circle` không thể thay thế được cho Shape và do vậy vi phạm LSP. Qua đó khiến cho hàm `DrawShape` vi phạm OCP. Do đó, có thể thấy rằng Vi phạm LSP là hậu quả của vi phạm OCP.

**`Square` và `Rectangle`, một vi phạm khôn khéo hơn**
Hiển nhiên, vẫn còn những ví dụ khác khôn khéo hơn rất nhiều ví dụ trên nhưng vẫn vi phạm LSP. Xem xét cách sử dụng class Rectangle mô tả ở ví dụ 10-2.

```C++
// Rectangle Class
class Rectangle
{
  public:
    void SetWidth(double w)  {itsWidth = w;}
    void SetHeight(double h) {itsHeight = h;}
    double getHeight() const {return itsHeight;}
    double getWidth() const  {return itsWidth;}
  private:
    Point  itsTopLeft;
    double itsWidth;
    double itsHeight;
}
```
Thử tưởng tượng ứng dụng của bạn chạy tốt ở nhiều màn hình. Giống như nhiều ứng dụng thành công khác, khách hàng không ngừng đưa ra yêu cầu. Một ngày, khách hàng yêu cầu xử lý hình vuông (squares).

Mọi người thường nói rằng thừa kế là một quan hệ IS-A (là một...). Nói một cách khác, nếu một loại object mới sinh ra mà thỏa mãn quan hệ IS-A với một loại object có sẵn, class của object mới sẽ được sinh ra từ class của object cũ.

Vậy là với logic thông thường, một hình vuông là một hình chữ nhật. Do vậy, một cách logic, class `Square` được sinh ra từ class `Rectangle` (Hình 10-1).

![markdown](https://github.com/manhnt7/Documentation/blob/main/image/Figure-10-1.png)

Ở đây chúng ta sử dụng quán hệ IS-A, đôi khi nó được coi như kiến thức cơ bản của lập trình hướng đối tượng: Một hình vuông là một hình chữ nhật, do vậy class Square cần được sinh ra từ class Rectangle. Tuy nhiên, cách nghĩ này đôi khi dẫn tới những vấn đề nghiêm trọng. Thông thường, chúng ta sẽ không thể nhìn ra vấn đề cho đến khi bắt tay code.

Gợi ý đầu tiên cho chúng ta hiểu được những sai lầm là hình vuông không cần đến cả chiều: dài và chiểu rộng (itsHeight, itsWidth). Dù nó thừa kế từ class Rectangle, lưu giữ thông tin ở hai biến là lãng phí. Trong nhiều trường hợp, việc lãng phí đó không quá nghiêm trọng. Tuy nhiên, nếu chúng ta phải tạo hàng trăm nghìn hình vuông (như chương trình CAD/CED), sự phí phạm này có thể trở nên nghiêm trọng.

Cứ cho rằng tại thời điểm hiện tại, chúng ta không lo lắng về hiệu quả sử dụng bộ nhớ, vẫn còn những vấn đề khác xảy ra do tạo class Square từ Rectangle. Square sẽ thừa kế hàm SetWidth và SetHeight. Hàm này không thích hợp với Square, vì chiều dài và rộng của hình vuông là một. Rõ ràng đây là một vấn đề nghiêm trọng. Tuy nhiên có một cách để giải quyết nó, chúng ta có thể ghi đè hàm SetWidth và SetHeight như sau.

```C++
void Square::SetWidth(double w)
{
  Rectangle::SetWidth(w);
  Rectangle::SetHeight(w);
}
void Square::SetHeight(double h)
{
  Rectangle::SetHeight(h);
  Rectangle::SetWidth(h);
}
```
Đến đây, khi ai đó thay đổi chiều rộng của hình vuông, chiều dài cũng thay đổi ngay lập tức. Và khi ai đó thay đổi chiều dài, chiều rộng cũng sẽ thay đổi theo. Đối tượng hình vuông vẫn duy trì được đặc tính hình học của nó

```C++
Square s;
s.SetWidth(1);   // Chiều dài cũng là 1
s.SetHeight(2);  // Cả chiều dài và rộng là 2
```

Tuy nhiên, xem xét hàm sau:

```C++
void f(Rectangle& r)
{
  r.SetWidth(32);   // Gọi hàm: Rectangle::SetWidth
}
```

Nếu chúng ta truyền tham số là một đối tượng của class Square, đối tượng đó sẽ bị hỏng bởi chiều dài sẽ không thay đổi. Rõ ràng, điều này vi phạm LSP. Hàm f không hoạt động với một class con của Rectangle. Lý do là bởi vì hàm SetWidth, SetHeight không được định nghĩa là hàm ảo virtual trong class Rectangle; do vậy, không có tính đa hình.

Chúng ta có thể sửa chữa một cách đơn giản. Tuy nhiên, việc tạo class con khiến chúng ta cần thay đổi class cơ sở, điều này thường dẫn đến các sai lầm về thiết kế. Cụ thể, nó vi phạm OCP. Chúng ta có thể thấy rằng, việc không định nghĩa hàm ảo virtual SetWidth, SetHeight là một sai lầm thiết kế, và chúng ta cần phải sửa ngay. Tuy nhiên, thật khó để nhận ra điều này ngay từ đầu khi mà SetWidth, SetHeight rõ ràng là những hàm phải có ngay từ đầu. Vì lý do nào chúng ta phải biến nó thành hàm ảo nếu chúng ta không dự báo sự tồn tại của Square.

Giả sử chúng ta chấp nhận thay đổi và sửa các class. Chúng ta sẽ làm như ví dụ 10-3.
```C++
class Rectangle
{
  public:
    virtual void SetWidth(double w) {itsWidth = w;}
    virtual void SetHeight(double h) {itsHeight = h;}
    double getWidth() const {return itsWidth;}
    double getHeight() const {return itsHeight;}
  private:
    Point itsTopLeft;
    double itsHeight;
    double itsWidth;
}

class Square : public Rectangle
{
  public:
    virtual void SetWidth(double w);
    virtual void SetHeight(double h);
}

void Square::SetWidth(double w)
{
  Rectangle::SetWidth(w);
  Rectangle::SetHeight(w);
}

void Square::SetHeight(double h)
{
  Rectangle::SetHeight(h);
  Rectangle::SetWidth(h);
}
```

**Vấn đề thực sự**
 `Square` và `Rectangle` bắt đầu hoạt động tốt. Bất kỳ điều gì bạn làm với đối tượng `Square`, đặc tính hình học của nó vẫn được duy trì. Tương tự với đối tượng `Rectangle`. Hơn nữa, bạn có thể truyền đối tượng của class `Square` vào những nơi nhận đầu vào là đối tượng của `Rectangle`, và nó vẫn hoạt động tốt.

Do vậy, chúng ta đi đến kết luận rằng thiết kế đã chính xác. Tuy nhiên, kết luận này có thể sai lầm. Một thiết kế nhất quán có thể không đúng với tất cả người dùng. Xem xét hàm g sau đây.
```C++
void g(Rectangle &r)
{
  r.SetWidth(5);
  r.SetHeight(4);
  assert(r.Area() == 20);
}
```
Hàm này gọi đến `SetWidth`, `SetHeight`, các hàm mà nó tin rằng của `Rectangle`. Hàm sẽ hoạt động tốt với hình chữ nhật, nhưng sẽ có lỗi khi làm với hình vuông. Do vậy, vấn đề ở đây là: Tác giả của g tin rằng việc thay đổi chiều rộng của hình chữ nhật không làm ảnh hưởng đến chiều dài.

Rõ ràng, hoàn toàn hợp lý khi giả thiết rằng thay đổi chiều rộng không ảnh hưởng đến chiều dài. Tuy nhiên, không phải tất cả các đối tượng được truyền vào như một hình chữ nhật thỏa mãn điều này. Nếu bạn truyền thực thể của class `Square` với hàm giống như g, nơi mà tác giả đã giả thiết điều trên, chương trình của bạn sẽ bị lỗi. Hàm g trở nên dễ vỡ trong mô hình `Rectangle/Square`.

Hàm `g` cho thấy tốn tại một hàm nhận đầu vào là thực thể của class `Rectangle`, nhưng lại không thể thực hiện được trong trường hợp của `Square`. Do vậy, trong các hàm như thế này, `Square` không thể thay thế được cho `Rectangle`. Quan hệ giữa `Square` và `Rectangle` vi phạm LSP.

Một vài người có thể cho rằng vấn đề nằm ở hàm `g`, trong đó tác giả đã không có quyền coi chiều dài và chiều rộng là độc lập với nhau. Tuy nhiên, tác giả của g có quyền không đồng ý. Hàm `g` nhận đầu vào là thực thể của class `Rectangle`. Có nhiều điều hiển nhiên, không thay đổi khi nói về hình chữ nhật, trong đó có một điều rõ ràng là chiều dài và chiều rộng phải độc lập nhau. Tác giả của g có đầy đủ quyền hành để kiểm tra điều đó. Do vậy, tác giả của `Square` mới là người phạm sai lầm khi vi phạm điều này.

Điều thú vị là tác giả của Square không vi phạm đặc tính của hình vuông. Nhưng bằng việc tạo Square từ Rectangle, anh ta vi phạm đặc tính của hình chữ nhật!

**Giá trị thuộc về bản chất**

LSP đưa chúng ta đến một quyết định quan trọng: *Một mô hình, xét trong một phạm vi cố định, không thể được đánh giá một cách đầy đủ*. Việc đánh giá mô hình chỉ có thể được thực hiện dựa theo cái nhìn của khách hàng. Ví dụ, khi chúng ta xem xét phiên bản cuối cùng của Square và Rectangle trong ví dụ trên, chúng ta có thể thấy chúng đều nhất quán và chính xác. Tuy nhiên, khi chúng ta nhìn nó từ vị trí của một lập trình viên, người đưa ra những giả thiết hợp lý về class cơ sở, mô hình của chúng ta bị phá vỡ.

Khi xem xét một thiết kế là đúng hay không, chúng ta không nên chỉ giới hạn trong một phạm vi nhất định. Chúng ta cần xem xét những giả thiết hợp lý có thể xảy ra bởi người dùng của thiết kế đó.

Ai biết được các giả thiết đó sẽ như thế nào? Hầu hết các giả thiết đó không thể dự đoán trước được. Thay vào đó, nếu chúng ta cố dự đoán tất cả, chúng ta sẽ rơi vào bẫy của "Sự phức tạp không cần thiết - Needless Complexity". Do vậy, cũng như tất cả các nguyên tắc khác, cách tốt nhất là làm thỏa mãn LSP ở mức tối thiểu, lờ đi tất cả những giả thiết có thể cho đến khi bắt đầu có dấu hiệu của một thiết kế dễ vỡ "Fragility".

**ISA liên quan đến hành vi**
Vậy điều gì đã diễn ra? Tại sao mối quan hệ có vẻ hợp lý giữa hình vuông và hình chữ nhật lại trở nên tồi tệ? Sau cùng, hình vuông có phải là hình chữ nhật? Quan hệ IS-A để làm gì?

Không như những gì tác giả của g suy nghĩ! Một hình vuông là một hình chữ nhật, nhưng từ quan điểm của g, một hình vuông tuyệt đối không là hình chữ nhật. Vì sao? Bởi hành vi của đối tượng hình vuông không thỏa mãn như mong muốn của hàm g về hành vi của hình chữ nhật. Về mặt hành vi, hình vuông không phải là hình chữ nhật, đó là hành vi mà nhà phát triển phần mềm cần nghĩ tới. LSP làm rõ điều đó hơn OCP, rằng quan hệ IS-A bao gồm hành vi mà có thể được yêu cầu một cách hợp lý từ phía khách hàng.

**Thiết kế dựa trên những giao kèo**
Nhiều lập trình viên có thể không thoải mái, với suy nghĩ về những giả thiết hợp lý. Làm cách nào để bạn biết điều khách hàng thực sự mong muốn? Có một kỹ thuật làm cho điều đó trở nên rõ ràng, từ đó thắt chặt LSP. Kỹ thuật đó được gọi lại thiết kế dựa trên những giao kèo - Design By Contract (DBC) được đưa ra bởi Betrand Meyer.

Sử dụng DBC, tác giả của class đưa ra giao kèo một cách rõ ràng. Giao kèo thông báo đến với tác giả của bất cứ dòng code nào sau này thông tin mà họ có thể tin tưởng được. Giao kèo được định nghĩa bởi điều kiện tiên quyết và điều kiện sau. Điều kiện tiên quyết cần đúng để phương thức có thể hoạt động. Đề hoàn thành, phương thức cần đảm bảo thỏa mãn điều kiện sau.

Chúng ta có thể coi điều kiện sau của hàm `Rectangle::SetWidth(double w)` như sau:

```C++
assert((itsWidth == w) && (itsHeight == old.itsHeight));
```
Trong ví dụ này, old là giá trị của hình chữ nhật trước khi gọi hàm SetWidth. Đến đây, luật về điều kiện tiên quyết và điều kiện sau của các lớp con được Betrand Meyer định nghĩa như sau:

*Một hành vi được định nghĩa lại (ở một class con) chỉ có thể thay thế điều kiện tiên quyết bởi một điều kiện bằng hoặc yếu hơn, và thay thế điều kiện sau bởi một điều kiện bằng hoặc mạnh hơn.*

Nói một cách khác, khi sử dụng đối tượng của một class dựa theo class cơ sở, người dùng chỉ biết về điều kiện tiên quyết và điều kiện sau. Do vậy, đối tượng của class con không được mong chờ người dùng tuân thủ điều kiện tiên quyết mạnh hơn class cơ sở. Chúng phải chấp nhận mọi thứ mà class cơ sở chấp nhận. Đồng thời, class con cần thỏa mãn mọi điều kiện sau của class cơ sở. Có nghĩa là, mọi hành vi và đầu ra của nó không được mâu thuẫn với những ràng buộc được định nghĩa ở lớp cơ sở. Người dùng của class cơ sở không cần phải lo lắng về đầu ra của class con.

Rõ ràng, điều kiện sau của hàm `Square::SetWidth(double w)` yếu hơn điều kiện sau của `Rectangle::SetWidth(double w)` bởi nó không bảo đảm được ràng buộc `(itsHeight == old.itsHeight)`. Do vậy, hàm `SetWidth` của Square vi phạm giao kèo của class cơ sở.

Một số ngôn ngữ nhất định, như Eiffel, có hỗ trợ trực tiếp cho điều kiện tiên quyết và điều kiện sau. Bạn có thể định nghĩa nó và hệ thống sẽ kiểm tra cho bạn khi thực thi chương trình. C++ và Java đều không có tính năng này. Ở các ngôn ngữ này, bạn cần chỉ định điều kiện bằng tay và tự viết cơ chế đảm bảo rằng luật của Meyer không bị vi phạm. Hơn nữa, sẽ thật cần thiết nếu bạn ghi chú lại các điều kiện này trong comment của từng hàm.

**Xác định giao kèo ở Unit Tests**

Giao kèo có thể chỉ định ở unit tests. Bằng việc kiểm tra hành vi của class, unit tests làm rõ ràng hành vi của class. Người dùng các hàm, class có thể dựa theo unit tests để xác định xem thay đổi nào là được phép khi làm việc với class đó.

# 5. ISP: The Interface-Segregation Principle

Nguyên tắc này giải quyết các nhược điểm của interfaces "béo". Các lớp có interfaces “béo” là các lớp có interfaces không kết dính. Nói cách khác, các interfaces của lớp có thể được chia thành các nhóm phương thức. Do đó, một số sẽ sử dụng groups các functions này và một số sẽ sử dụng functions khác. 

**Interface Pollution**

Xem xét một hệ thống bảo mật. Trong hệ thống này, có các đối tượng Door có thể được khóa và mở, và biết chúng đang mở hay đóng. (xem ví dụ 12-1)

Listing 12-1
Security Door

```C++
class Door
{
 public:
 virtual void Lock() = 0;
 virtual void Unlock() = 0;
 virtual bool IsDoorOpen() = 0;
};
```

Đây là một abstract class có thể sử dụng các đối tượng phù hợp với `Door` interface, mà không cần phải phụ thuộc vào việc triển khai cụ thể của Door.
Bây giờ hãy xem xét rằng một implementation như vậy, `TimedDoor`, cần phát ra âm thanh báo động khi cửa mở quá lâu. Để làm điều này, đối tượng `TimedDoor` giao tiếp với một đối tượng khác được gọi là `Timer`.

Listing 12-2

```C++
class Timer
{
 public:
 void Register(int timeout, TimerClient* client);
};
class TimerClient
{
 public:
 virtual void TimeOut() = 0;
};
```

Khi một đối tượng muốn được thông báo về thời gian chờ, nó gọi function `Register` của `Timer`. Các đối số của hàm này là thời gian hết giờ. và một con trỏ đến một đối tượng `TimerClient` có hàm `TimeOut` sẽ được gọi khi hết thời gian chờ.

