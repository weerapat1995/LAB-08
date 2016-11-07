#ใบงานที่ 8
##การเปลี่ยนทิศทางการทำงานของโปรแกรม

##1). การเปลี่ยนทิศทางการทำงานของโปรแกรม

###1.1). การเปลี่ยนทิศทางแบบไม่มีเงื่อนไข

การเปลี่ยนทิศทางแบบไม่มีเงื่อนไขในภาษา C# มีประโยชน์ตรงที่สามารถเปลี่ยนทิศทางได้ทันต่อความต้องการ เนื่องจากในโปรแกรมบางประเภทต้องมีการตอบสนองแบบทันทีทันใด เช่นในกรณีที่เกิด exception ต่างๆ หรือต้องการออกนอก loop ต่างๆ โดยไม่สามารถใช้วิธีการตามปกติได้ การเปลี่ยนทิศทางการทำงานของโปรแกรมแบบไม่มีเงื่อนไข มีหลายคำสั่ง เช่น ```goto```, ```try…catch```, ```throw```, ```break```, ```continue``` และ ```return```
####1.1.1. คำสั่ง goto
คำสั่ง goto ใช้เพื่อกระโดดไปทำงานยังตำแหน่งที่ต้องการ 
**ตัวอย่าง** โปรแกรมที่ใช้คำสั่ง goto
```csharp
1 using System;
2 public class GotoTest
3 {
4      static void Main(string[] args)
5      {
6         Console.WriteLine("Line 1");
7         Console.WriteLine("Line 2");
8         Console.WriteLine("Line 3");
9         line4:
10        Console.WriteLine("Line 4");
11        Console.WriteLine("Line 5");
12        Console.WriteLine("Line 6");
13        goto line10;
14        Console.WriteLine("Line 7");
15        Console.WriteLine("Line 8");
16        Console.WriteLine("Line 9");
17        line10:
18        Console.WriteLine("Line 10");
19    }
20 }
```
ผลที่ได้
```
Line 1
Line 2
Line 3
Line 4
Line 5
Line 6
Line 10
```
การทดลอง
ให้นักศึกษา แก้ไขดัดแปลงโปรแกรม โดยใช้คำสั่ง goto และให้มีเอาต์พุตดังนี้
```
Line 1
Line 4
Line 5
Line 2
Line 9
```

**Code**

```C#
using System;


namespace Lab8
{
    class Program
    {
        static void Main(string[] args)
        {
        line1: Console.WriteLine("Line 1");
            goto line4;
        line2: Console.WriteLine("Line 2");
            goto line9;
        line3: Console.WriteLine("Line 3");
        line4: Console.WriteLine("Line 4");
        line5: Console.WriteLine("Line 5");
            goto line2;
        line6: Console.WriteLine("Line 6");
        line7: Console.WriteLine("Line 7");
        line8: Console.WriteLine("Line 8");
        line9: Console.WriteLine("Line 9");
            goto lineEnd;
        line10: Console.WriteLine("Line 10");
        lineEnd:;
        }
    }
    }
```

###1.1.2. try…catch…finally
ประโยค ```try…catch…finally``` ใช้สำหรับการดักจับและจัดการข้อผิดพลาดของโปรแกรม ทั้งขณะทำงาน (Run Time Process) หรือในขณะเริ่มต้นทำงาน (Init Process) โดยเราจะวางคำสั่งที่คาดการว่าจะเกิดข้อผิดพลาดขึ้นไว้ในบล็อกของ ```Try``` และวางส่วนจัดการข้อผิดพลาดไว้ในบล็อกของ ```catch``` และถ้ามีการดำเนินการใดๆ ที่ต้องทำทั้งในกรณีที่มีและไม่มีข้อผิดพลาด ก็จะใส่ไว้ในบล็อกของ ```Finally``` ในคำสั่งนี้สามารถเขียนบล็อกของ ```catch``` ได้หลายบล็อก คำสั่งนี้มีประโยชน์มากในการทำงานกับระบบอินเตอร์เน็ต โดยเฉพาะในกรณีที่การเชื่อมต่อไม่เสถียร เพราะจะช่วยป้องกันการค้างของโปรแกรมของเราขณะเรียกข้อมูลจาก network ได้
**ตัวอย่าง** โปรแกรมที่ไม่ได้ใช้คำสั่ง ```try…catch…finally```
``` csharp
using System;
public class TryCatch
{
    static void Main(string[] args)
    {
        object o2 = null;
        int i2 = (int)o2;   // Error
    }
}
```
**ผลที่ได้**
โปรแกรมจะค้างและปรากฏข้อความต่อไปนี้บนหน้าจอ
```
Unhandled Exception: System.NullReferenceException: 
Object reference not set to an instance of an object. 
at TryCatch.Main(String[] args) in…..Program.cs:line 7
```
เนื่องจากมีการส่งค่าที่เป็น ```null``` ให้กับตัวแปร i2 จึงเกิด error ดังกล่าวขึ้นในขณะ run time ซึ่งจะเห็นว่า เราสามารถรันโปรแกรมนี้ได้ เนื่องจาก compiler ตรวจไม่พบข้อผิดพลาดในขณะคอมไพล์

วิธีการดักจับและแก้ไขข้อผิดพลาด ทำได้โดยการใช้คำสั่ง ```try…catch``` กับส่วนของโปรแกรมที่คาดว่าอาจจะเกิดข้อผิดพลาด ซึ่งจากโปรแกรมข้างบนนั้น compiler ฟ้องว่ามีข้อผิดพลาดเกิดขึ้นในบรรทัดที่ 7 ดังนั้นสามารถแก้ไขโปรแกรมได้เป็นดังนี้

**ตัวอย่าง** โปรแกรมที่ไม่ได้ใช้คำสั่ง ```try…catch…finally```
```csharp
using System;
public class TryCatch
{
    static void Main(string[] args)
    {
        object o2 = null;
        try
        {
            int i2 = (int)o2;
            Console.WriteLine("i2 = {0}", i2);
        }
        catch
        {
            Console.WriteLine("Error, null object assignment.");
        }
    }
 }
```
**ผลที่ได้**
โปรแกรมทำงานได้ตามปกติ และปรากฏ ข้อความต่อไปนี้ โดยที่ข้ามคำสั่งบรรทัดที่ 10
```
Error, null object assignment.
```

ในคำสั่ง ```catch``` นั้น เราสามารถใส่ parameter ซึ่งเป็นประเภทของข้อผิดพลาดได้ด้วย เช่น  ```NullReferenceException``` เพื่อดักจับการส่งค่า ```null``` ให้ตัวแปร หรือ ```DivideByZeroException``` ไว้คอยดักจับ ในกรณีที่มีการหารด้วยค่าศูนย์ เป็นต้น โดยมีรูปแบบการใช้งานดังตัวอย่าง
ตัวอย่าง การดักจับข้อผิดพลาดหลายๆ รูปแบบ
```csharp
using System;
public class TryCatch
{
    static void Main(string[] args)
    {
        int a = 0;
        try
        {
             
            Console.WriteLine(100/a);
        }
        catch(NullReferenceException e)
        {
            Console.WriteLine(e.Message);
        }
        catch (DivideByZeroException e)
        {
            Console.WriteLine(e.Message);
        }
    }
 }
```
###การทดลอง
ให้นักศึกษาเขียนโปรแกรมเพื่อทดสอบว่าโปรแกรมต่อไปนี้มีความผิดพลาดในการทำงานหรือไม่ ถ้ามี ให้นักศึกษาเขียนคำสั่ง try…catch เพิ่มเข้าไป โดยเลือกประเภทของ exception จาก reference ท้ายใบงาน

###1.
```csharp
using System;
public class TryCatch
{
    static void Main(string[] args)
    {
        int a = int.MaxValue;  
        a *= 2;
        Console.WriteLine(a);
     }
 }
```

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.1.png)

###2.
``` csharp
using System;
public class TryCatch
{
    static void Main(string[] args)
    {
        int a = 0;
        int b = 10;  
        b /= a;
        Console.WriteLine(a);
     }
 }
```

**แก้ไขเป็น**

```C#
using System;


namespace Lab8._1
{
    class Program
    {
        static void Main(string[] args)
        {
            object o2 = null;
            try
            {
                int a = 0;
                int b = 10;
                b /= a;
                Console.WriteLine(a);
            }
            catch(DivideByZeroException e)
            {
                Console.WriteLine(e.Message);
            }
        }
    }
}
```

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.2.png)

###3.
``` csharp
using System;
public class TryCatch
{
    static void Main(string[] args)
    {
        int value = 800000000;
        checked // check for overflow
        {
                int square = value * value;
                Console.WriteLine("{0} ^ 2 = {1}", value, square);
        }
     }
 }
```

**แก้ไขเป็น**

```C#
using System;


namespace Lab8._1
{
    class Program
    {
        static void Main(string[] args)
        {
            try
            {
                int value = 800000000;
                checked // check for overflow
                {
                    int square = value * value;
                    Console.WriteLine("{0} ^ 2 = {1}", value, square);
                }
            }
            catch(OverflowException e)
            {
                Console.WriteLine(e.Message);
            }
        }
    }
}
```

**ผลที่ได้

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.3.png)


###1.1.3. คำสั่ง ```throw```

คำสั่ง ```throw``` ใช้เพื่อเปลี่ยนเส้นทางการทำงานของโปรแกรมโดยเจาะจง exception เป้าหมาย

**ตัวอย่าง** การดักจับข้อผิดพลาดหลายๆ รูปแบบ
```csharp
using System;
using System.IO;
public class ExceptionLearning
{
    public static void Main()
    {
        int a = 10;
        int b = 20;
        int c = add(a, b);
    }
    private static int add(int a, int b)
    {
        throw new NotImplementedException();
    }
}
````
###การทดลอง ชนิดของ exception

ให้เปลี่ยนชนิดของการ throw exception ในบรรทัดที่ 34 เป็น exception ดังต่อไปนี้ แล้วอธิบายผลที่ได้
```
1.	DivideByZeroException
2.	NullReferenceException
3.	FileNotFoundException
4.	FormatException
```
```csharp
using System;
using System.IO;
public class ExceptionLearning
{
    public static void Main()
    {
        int a = 10;
        int b = 20;
        int c ;
        try
        {
            c = div(a, b);
        }
        catch (DivideByZeroException e)
        {

            Console.WriteLine("DivideByZeroException");
            Console.WriteLine(e.Message);
        }
        catch (NullReferenceException e) 
        {
            Console.WriteLine("NullReferenceException");
            Console.WriteLine(e.Message);
        
        }
        catch (Exception e)
        {
            Console.WriteLine("Exception");
            Console.WriteLine(e.Message);
        }
    }
    private static int div(int a, int b)
    {
        throw new _____________________();
    }
 }
````

1.	DivideByZeroException

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.4.png)

2.	NullReferenceException

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.5.png)

3.	FileNotFoundException

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.6.png)

4.	FormatException

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.7.png)


###เรื่องของ exception นี้ศึกษาเพิ่มเติมได้ [ที่นี่](http://msdn.microsoft.com/en-us/library/vstudio/2w8f0bss%28v=vs.100%29.aspx)

##1.2.	การเปลี่ยนทิศทางแบบมีเงื่อนไข (Conditional Branching)

###1.2.1.	คำสั่ง ```if```

คำสั่ง ```if``` เป็นคำสั่งที่ใช้เปลี่ยนทิศทางการทำงานของโปรแกรมตามเงื่อนไข โดยค่าที่นำมาเป็นเงื่อนไขในการตัดสินใจ จะต้องมีชนิดเป็น boolean (ซึ่งมีค่าเป็น ```true``` หรือ ```false```) เท่านั้น

**รูปแบบของคำสั่ง ```if```**

**1. แบบมีคำสั่งเดียว**	

```csharp
    if ( condition )    
        statement ;  
```

**2.แบบมีหลายคำสั่ง (เป็นบล็อก)**
```csharp
     if ( condition )
    {    // begin of block
        statement_1 ;  
        statement_2 ;  
        ...  
        statement_3 ;  
    }    // end of block
```

**ตัวอย่าง** การใช้งานคำสั่ง ```if```
```csharp
using System;
using System.IO;
public class IfLearning
{
    public static void Main()
    {
        int a = 2;
        if (a == 2)
        {
            Console.WriteLine("execute this line");
        }
        if (a < 2)
        {
            Console.WriteLine("execute this line too");
        }
        Console.WriteLine("execute next line");
    }
}
```
ผลจากการทำงานของโปรแกรม
```
execute this line
execute next line
```
ในบล็อกของคำสั่ง ```if``` นั้น บรรทัดที่ถูกเรียกทำงานคือบรรทัดที่มีเงื่อนไขเป็น ```true``` เท่านั้น บรรทัดที่เงื่อนไขของ ```if``` มีค่าเป็น ```false``` จะไม่ถูกเรียกทำงาน โดยการตัดสินใจจะเป็นอิสะต่อกัน คือคำสั่ง ```if``` บล็อกหลังไม่ได้รับผลกระทบใดๆ จาก ```if``` ในบล็อกแรก เช่นเดียวกับบรรทัดที่ 16 ของโปรแกรม ซึ่งไม่มีคำสั่งใดๆ มาควบคุมลำดับการทำงาน มันจึงถูกเรียกทำงานตามปกติ

###การทดลอง

ให้เขียนโปรแกรมสุ่มตัวเลข (จากใบงานที่ 7) แล้วใช้คำสั่ง ```if``` อย่างเดียวเท่านั้น โดยมีเงื่อนไขต่อไปนี้

1.	ถ้าค่าที่ผู้ใช้ป้อน มากกว่า ค่าที่สุ่มมาได้ ให้พิมพ์ ```“Too Hight, You loss!!”``` ออกทางหน้าจอ
2.	ถ้าค่าที่ผู้ใช้ป้อน น้อยกว่า ค่าที่สุ่มมาได้ ให้พิมพ์ ```“Too Low, You loss!!”```ออกทางหน้าจอ
3.	ถ้าค่าที่ผู้ใช้ป้อน เท่ากับ ค่าที่สุ่มมาได้ ให้พิมพ์ ```“Okay, You win!!”``` ออกทางหน้าจอ

**Code**

```C#
using System;

namespace Lab8._3
{
    class Program
    {
        static void Main(string[] args)
        {
            Random random = new Random();
            int randomNumber = random.Next(0, 100);
            Console.WriteLine("Number Random : " + randomNumber);
            Console.Write("Please Enter Number : ");
            int a = Convert.ToInt32(Console.ReadLine());
            if (a > randomNumber)
            {
                Console.WriteLine("Too Hight, You loss!!");
            }
            if (a < randomNumber)
            {
                Console.WriteLine("Too Low, You loss!!");
            }
            if (a == randomNumber)
            {
                Console.WriteLine("Okay, You win!!");
            }

        }
    }
}
```
**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.8.png)

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.9.png)

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.10.png)

###1.2.2.	คำสั่ง ```if…else```

เงื่อนไขที่เป็นไปได้ของคำสั่งในการตัดสินใจมีสองทางเสมอ (true และ false) ที่ผ่านมา เราจะเห็นว่า คำสั่ง if เป็นคำสั่งที่เลือกทำเพียงทางเดียว (เฉพาะในกรณีที่เงื่อนไขเป็น true เท่านั้น) หากต้องการให้โปรแกรมทำงานทั้งกรณีที่เงื่อนไขเป็น true และ false เราต้องใช้คำสั่ง if…else โดยมีรูปแบบดังนี้
รูปแบบของคำสั่ง if…else

```cs
    if (condition)
    {
        statement;  // execute when condition = true
    }
    else
    {   
        statement; // execute when condition = false
    }
```
**ตัวอย่าง** การใช้งานคำสั่ง ``if…else``

```cs
using System;
using System.IO;
public class IfLearning
{
    public static void Main()
    {
        int a = 2;
        if (a == 2)
        {
            Console.WriteLine("execute this line");
        }
        else
        {
            Console.WriteLine("execute another line too");
        }
        Console.WriteLine("this line is always execute");
    }
}
```
ผลจากการทำงานของโปรแกรม
```
execute this line
this line is always execute
```
**การทดลอง**

ให้เขียนโปรแกรมสุ่มตัวเลข (จากใบงานที่ 7) แล้วใช้คำสั่ง ```if…else``` โดยมีเงื่อนไขต่อไปนี้

1. ถ้าค่าที่ผู้ใช้ป้อน เท่ากับ ค่าที่สุ่มมาได้ ให้พิมพ์ ```“Hooray, You win!!”``` ออกทางหน้าจอ มิฉะนั้นให้พิมพ์คำว่า ```“Sorry, You loss!!”```

**Code**

```C#
using System;

namespace Lab8._3
{
    class Program
    {
        static void Main(string[] args)
        {
            Random random = new Random();
            int randomNumber = random.Next(0, 100);
            Console.WriteLine("Number Random : " + randomNumber);
            Console.Write("Please Enter Number : ");
            int a = Convert.ToInt32(Console.ReadLine());
            if (a == randomNumber)
            {
                Console.WriteLine("Hooray, You win!!");
            }
            else
            {
                Console.WriteLine("Sorry, You loss!!");
            }
        }
    }
}
```

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.11.png)

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.12.png)


###1.2.3.	คำสั่ง ```if``` ซ้อนกัน (nested if)
คำสั่ง ```if``` สามารถเขียนซ้อนกันเป็นชั้นได้ เรียกว่า nested if มีรูปแบบดังนี้
####รูปแบบของคำสั่ง nested if
```cs
    if (condition)
    {
        if (condition)  // nested if
        {
            ...;   
        }
    }
```
**ตัวอย่าง** การใช้งานคำสั่ง nested if
```cs
using System;
using System.IO;
public class IfLearning
{
    public static void Main()
    {
        int a = 10;
        int b = 20;
        if (a == 10)
        {
            if (b == 20)
            {
                Console.WriteLine("a = 10 and b = 20");
            }
            if (b != 20)
            {
                Console.WriteLine("a = 10 and b != 20");
            }
        }
    }
}
```
ผลจากการทำงานของโปรแกรม
```
a = 10 and b = 20
```
**ข้อสังเกตุ** คำสั่ง nested if เปรียบเทียบได้กับการ AND กันของเงื่อนไขในระดับต่างๆ 

###1.2.4.	คำสั่ง ```if…else…if```

ในบางกรณีที่มีการตัดสินใจในหลายทางเลือก เราอาจใช้คำสั่ง ```if…else…if``` เรียงต่อกันไปเรื่อยๆ ตัวอย่างเช่นโปรแกรมการตัดเกรดที่มีหลายระดับ เป็นต้น

**ตัวอย่าง** การใช้งานคำสั่ง ```if…else…if```
```cs
using System;
using System.IO;
public class IfLearning
{
    public static void Main()
    {
        int point = 68;
        if (point < 50)
            Console.WriteLine("Grade F");
        else if (point < 60)
            Console.WriteLine("Grade D");
        else if (point < 70)
            Console.WriteLine("Grade C");
        else if (point < 80)
            Console.WriteLine("Grade B");
        else
            Console.WriteLine("Grade A");
    }
}
```
ผลจากการทำงานของโปรแกรม
```
Grade C
```
**การทดลอง**

ให้เขียนโปรแกรมตัดเกรดโดยการสุ่มตัวเลขจากใบงานที่ 7 แล้วใช้คำสั่ง if…else...if เพื่อการตัดเกรดโดยมีเงื่อนไขต่อไปนี้

1.	ระดับคะแนนที่จะนำมาตัดเกรด ได้จากการสุ่ม มีค่าจาก 0 ถึง 100
2.	ตัดเกรดโดยใช้เกณฑ์ตามตารางต่อไปนี้ 

ระดับคะแนน|เกรด
:----|:----:
80-100|	A
75-79|	B+
70-74|	B
65-69|	C+
60-64|	C
55-59|	D+
50-54|	D
0-49|	F

3. รูปแบบการพิมพ์คือ score: [sss] grade: [gg] เมื่อ sss คือคะแนน และ gg คือ เกรดที่ได้

**Code**

```C#
using System;

namespace Lab8._3
{
    class Program
    {
        static void Main(string[] args)
        {
            Random random = new Random();
            int sss = random.Next(0, 100);
            
            if (sss < 50)
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : F");
            }
            else if (sss < 55)
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : D");
            }
            else if (sss < 60)
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : D+");
            }
            else if (sss < 65)
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : C");
            }
            else if (sss < 70)
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : C+");
            }
            else if (sss < 75)
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : B");
            }
            else if (sss < 80)
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : B+");
            }
            else
            {
                Console.WriteLine("Score : " + sss);
                Console.WriteLine("Grade : A");
            }
        }
    }
}
```

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.15.png)

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.16.png)



###1.2.5. คำสั่ง ```switch```

ในกรณีที่มีทางเลือกในการตัดสินใจเป็นจำนวนมาก ไม่เป็นการสะดวกที่จะเขียนเป็นโปรแกรมยาวๆ เช่นในกรณีของคำสั่ง if…else…if ภาษา C# มีคำสั่งตัดสินใจเลือกทิศทางของโปรแกรมแบบหลายทางเลือกให้ใช้คือคำสั่ง switch ซึ่งรูปแบบการใช้งาน ดังนี้

**รูปแบบของคำสั่ง ```switch```**
```cs
    switch(<expression>) {
        case <value> : <statement>
        case <value> : <statement>
        case <value> : <statement>
        ..........................
        [default : <statement>]
    } 
```
ในภาษา C# นั้น ยอมให้นิพจน์ (constant-expression) เป็นแบบจํานวนเต็ม(integer)  แบบอักขระ(char) และ แบบข้อความ(string) 
ตัวอย่าง การใช้คำสั่ง switch 

```cs
using System;
using System.IO;
public class switchLearning
{
    public static void Main()
    {
        Console.Write("Input your grade (A, B, C, D or F) : ");
        string gradeString =  Console.ReadLine();
        string message;
        switch (gradeString.ToUpper())
        {
            case "A":
                message = "Excellent";
                break;
            case "B":
                message = "Good";
                break;
            case "C":
                message = "Cool";
                break;
            case "D":
                message = "Try";
                break;
            case "F":
                message = "Get out!!";
                break;
            default:
                message = "Incorrect grade";
                break;
        }
        Console.WriteLine(message); 
    }
}
```

ผลจากการทำงานของโปรแกรม
```
Input your grade (A, B, C, D or F) : B
Good
```
**การทดลอง** เรื่องคำสั่ง switch
ให้เขียนโปรแกรมรับค่าชื่อของวัน แล้วพิมพ์ข้อความออกทางหน้าจอ ดังตัวอย่าง
```
Input day name : sun
sun is Sunday, color Red
```
**ตารางกำหนดชื่อและสีประจำวัน**

Input ที่รับได้	|ชื่อวัน|	สี
---|:---:|:---:
sun|	Sunday	|Red
mon|	Monday	|Yellow
tue|	Tuesday	|Pink
wed|	Wednesday	|Green
thu|	Thursday	|Orange
fri|	Friday	|Blue
sat|	Saturday	|Purple
อื่นๆ|	 ---|	---

**Code**

```C#
using System;

namespace Lab8._3
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Input day name : ");
            string day = Console.ReadLine();
            string message;
            switch(day.ToUpper())
            {
                case "SUN":
                    message = "sun is Sunday ,color Red";
                    break;
                case "MON":
                    message = "mon is Monday ,color Red";
                    break;
                case "TUE":
                    message = "tue is Tuesday ,color Red";
                    break;
                case "WED":
                    message = "wed is Wednesday ,color Red";
                    break;
                case "FRI":
                    message = "fri is Friday ,color Red";
                    break;
                case "SAT":
                    message = "sat is Saturday ,color Red";
                    break;
                default:
                    message = "sat is --- ,color ---";
                    break;
            }
            Console.WriteLine(message);
        }
    }
}

```

**ผลที่ได้**

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.13.png)

![](https://github.com/weerapat1995/LAB-08/blob/master/imgs/8.14.png)

##Reference
เนื้อหาในส่วนนี้เป็นอ้างอิงสำหรับการเขียนโปรแกรม

###Exceptions

Exception | Condition
-----|-------
ArgumentException| A non-null argument that is passed to a method is invalid. 
ArgumentNullException |An argument that is passed to a method is null. 
ArgumentOutOfRangeException|An argument is outside the range of valid values. 
DirectoryNotFoundException|Part of a directory path is not valid. 
DivideByZeroException|The denominator in an integer or Decimal division operation is zero. 
DriveNotFoundException|A drive is unavailable or does not exist. 
FileNotFoundException|A file does not exist. 
FormatException|A value is not in an appropriate format to be converted from a string by a conversion method such as Parse. 
IndexOutOfRangeException|An index is outside the bounds of an array or collection. 
InvalidOperationException|A method call is invalid in an object's current state. 
KeyNotFoundException|The specified key for accessing a member in a collection cannot be found. 
NotImplementedException|A method or operation is not implemented. 
NotSupportedException|A method or operation is not supported. 
ObjectDisposedException|An operation is performed on an object that has been disposed. 
OverflowException|An arithmetic, casting, or conversion operation results in an overflow. 
PathTooLongException|A path or file name exceeds the maximum system-defined length. 
PlatformNotSupportedException|The operation is not supported on the current platform. 
RankException|An array with the wrong number of dimensions is passed to a method. 
TimeoutException|The time interval allotted to an operation has expired. 
UriFormatException|An invalid Uniform Resource Identifier (URI) is used. 

