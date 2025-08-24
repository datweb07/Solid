### 5 nguyên tắc Solid trong OOP
1. Single Responsibility Principle -> Mỗi class chỉ nên có ***một trách nhiệm (nhiệm vụ) duy nhất***
2. Open/Closed Principle (Open for extension, closed for modification) -> Class nên ***mở rộng nhưng không thay đổi trực tiếp***
3. Liskov Substitution Principle -> Lớp con có thể ***thay thế lớp cha*** mà không làm thay đổi tính đúng đắn của chương trình
4. Interface Segregation Principle -> Nên tạo nhiều ***interface chuyên biệt*** thay vì chỉ một một interface quá to
5. Dependency Inversion Principle -> Module cấp cao không nên phụ thuộc vào module cấp thấp, cả hai nên phụ thuộc vào ***abstraction (interface/abstract class)***

---

### Ví dụ minh họa
**1. Single Responsibility Principle**
```csharp
// Vi phạm: class vừa quản lý dữ liệu, vừa in ra
class Report
{
    public string Title { get; set; }
    public string Content { get; set; }

    public void PrintReport()
    {
        Console.WriteLine($"{Title}: {Content}");
    }
}

// Tuân thủ: tách riêng nhiệm vụ
class Report
{
    public string Title { get; set; }
    public string Content { get; set; }
}

class ReportPrinter
{
    public void Print(Report report)
    {
        Console.WriteLine($"{report.Title}: {report.Content}");
    }
}
```
---

**2. Open/Closed Principle**
```csharp
// Vi phạm: phải sửa code nếu thêm loại hình thanh toán
class Payment
{
    public void Pay(string method)
    {
        if (method == "Cash")
            Console.WriteLine("Thanh toán tiền mặt");
        else if (method == "CreditCard")
            Console.WriteLine("Thanh toán thẻ tín dụng");
    }
}

// Tuân thủ OCP: dễ mở rộng bằng class mới
abstract class Payment
{
    public abstract void Pay();
}

class CashPayment : Payment
{
    public override void Pay() => Console.WriteLine("Thanh toán tiền mặt");
}

class CreditCardPayment : Payment
{
    public override void Pay() => Console.WriteLine("Thanh toán thẻ tín dụng");
}
```
---
**3. Liskov Substitution Principle**
```csharp
// Vi phạm: lớp con thay đổi hành vi lớp cha
class Bird
{
    public virtual void Fly() => Console.WriteLine("Bay trên trời");
}

class Penguin : Bird
{
    public override void Fly() => throw new Exception("Chim cánh cụt không bay!");
}

// Tuân thủ: phân loại hợp lý
abstract class Bird { }
class FlyingBird : Bird
{
    public void Fly() => Console.WriteLine("Bay trên trời");
}
class Penguin : Bird
{
    public void Swim() => Console.WriteLine("Bơi dưới nước");
}
```
---
**4. Interface Segregation Principle**
```csharp
// Vi phạm: interface quá to
interface IWorker
{
    void Work();
    void Eat();
}

// Tách nhỏ
interface IWork
{
    void Work();
}

interface IEat
{
    void Eat();
}

class Human : IWork, IEat
{
    public void Work() => Console.WriteLine("Người làm việc");
    public void Eat() => Console.WriteLine("Người ăn cơm");
}

class Robot : IWork
{
    public void Work() => Console.WriteLine("Robot làm việc không cần ăn");
}
```
---
**5. Dependency Inversion Principle**
```csharp
// Vi phạm: phụ thuộc trực tiếp vào class cụ thể
class EmailService
{
    public void Send(string message) => Console.WriteLine($"Gửi email: {message}");
}

class Notification
{
    private EmailService _email = new EmailService();

    public void SendNotification(string msg)
    {
        _email.Send(msg);
    }
}

// Tuân thủ: phụ thuộc abstraction
interface IMessageService
{
    void Send(string message);
}

class EmailService : IMessageService
{
    public void Send(string message) => Console.WriteLine($"Gửi email: {message}");
}

class SmsService : IMessageService
{
    public void Send(string message) => Console.WriteLine($"Gửi SMS: {message}");
}

class Notification
{
    private readonly IMessageService _service;

    public Notification(IMessageService service)
    {
        _service = service;
    }

    public void SendNotification(string msg)
    {
        _service.Send(msg);
    }
}
```
