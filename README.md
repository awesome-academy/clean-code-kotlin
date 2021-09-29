Original Repository: [ryanmcdermott/clean-code-java](https://github.com/leonardolemie/clean-code-java)

# clean-code-kotlin

## Mục lục:
  1. [Giới thiệu](#giới-thiệu)
  2. [Biến](#biến)
  3. [Hàm](#hàm)
  4. [Đối tượng và Cấu trúc dữ liệu](#đối-tượng-và-cấu-trúc-dữ-liệu)
  5. [Lớp](#lớp)
  6. [SOLID](#solid)
  7. [Testing](#testing)
  8. [Xử lí đồng thời](#xử-lí-đồng-thời)
  9. [Xử lí lỗi](#xử-lí-lỗi)
  10. [Định dạng](#định-dạng)
  11. [Viết chú thích](#viết-chú-thích)
  12. [Các ngôn ngữ khác](#các-ngôn-ngữ-khác)
  
## Giới thiệu

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](http://www.osnews.com/images/comics/wtfm.jpg)

Những nguyên tắc kỹ thuật phần mềm, từ cuốn sách
[*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882),
áp dụng cho ngôn ngữ Kotlin. Đây không phải là một hướng dẫn về cách viết code Kotlin mà là hướng dẫn về cách viết các đoạn code dễ đọc hiểu, tái sử dụng và tái cấu trúc được trong Kotlin.

Không phải mọi nguyên tắc ở đây phải được tuân thủ một cách nghiêm ngặt,
và thậm chí chỉ có một ít trong số đó được sử dụng phổ biến. Ở đây, nó chỉ là một
hướng dẫn - không hơn không kém, nhưng chúng được hệ thống hóa thông qua kinh
nghiệm thu thập được qua nhiều năm của các tác giả của cuốn sách [*Clean Code*](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882)

Ngành kỹ thuật phần mềm chỉ phát triển được hơn 50 năm, và chúng ta vẫn
đang học rất nhiều. Một khi kiến trúc phần mềm trở thành phổ biến, có lẽ sau đó
chúng ta sẽ có thêm nhiều luật lệ khó hơn phải tuân theo. Còn giờ đây,
hãy để những hướng dẫn này như là một tiêu chuẩn để đánh giá chất lượng các đoạn
code Kotlin mà bạn và team của bạn tạo ra.

Biết những hướng dẫn này thôi sẽ không thể ngay lập tức làm bạn trở thành một
lập trình viên phần mềm tốt hơn được, và làm việc với chúng trong nhiều năm
cũng không có nghĩa bạn sẽ không gặp bất cứ sai lầm nào. Mỗi đoạn code bắt đầu
như một bản thảo đầu tiên, giống như đất sét được nặn nhào và cho tới cuối cùng
thì nó sẽ lộ diện hình hài. Cuối cùng, chúng ta gọt tỉa những khuyết điểm khi
chúng ta xem xét lại nó cùng với các đồng nghiệp.
Đừng để bản thân bạn bị đánh bại bởi những bản thảo đầu tiên,
thứ mà vẫn cần phải được chỉnh sửa. Thay vào đó hãy đánh bại những dòng code.

## Biến
### Sử dụng tên biến có nghĩa và dễ phát âm

**Không tốt:**
```kotlin
val yyyymmdstr = SimpleDateFormat("YYYY/MM/DD").format(Date())
```

**Tốt:**
```kotlin
val currentDate = SimpleDateFormat("YYYY/MM/DD").format(Date())
```

**[⬆ về đầu trang](#mục-lục)**

### Sử dụng cùng từ vựng cho cùng loại biến

**Không tốt:**

```kotlin
getUserInfo()
getClientData()
getCustomerRecord()
```

**Tốt:**
```kotlin
getUser()
```

**[⬆ về đầu trang](#mục-lục)**

### Sử dụng các tên có thể tìm kiếm được
Chúng ta sẽ đọc code nhiều hơn là viết chúng. Điều quan trọng là code chúng ta
viết có thể đọc được và tìm kiếm được. Việc đặt tên các biến *không* có ngữ
nghĩa so với chương trình, chúng ta có thể sẽ làm người đọc code bị tổn thương
tinh thần.
Hãy làm cho các tên biến của bạn có thể tìm kiếm được.

**Không tốt:**
```kotlin
// 86400000 là cái quái gì thế?
setTimeout(blastOff, 86400000)

```

**Tốt:**
```kotlin
// Khai báo chúng như một biến hằng global.
const val MILLISECONDS_IN_A_DAY = 86400000

setTimeout(blastOff, MILLISECONDS_IN_A_DAY)

```

**[⬆ về đầu trang](#mục-lục)**

### Sử dụng những biến có thể giải thích được
**Không tốt:**
```kotlin
val address = "One Infinite Loop, Cupertino 95014"
val cityZipCodeRegex = "[\\-\\+\\.\\^:,]".toRegex()

saveCityZipCode(address.split(cityZipCodeRegex)[0] , address.split(cityZipCodeRegex)[1])
```

**Tốt:**
```kotlin
val address = "One Infinite Loop, Cupertino 95014"
val cityZipCodeRegex = "[\\-\\+\\.\\^:,]".toRegex()
val city = address.split(cityZipCodeRegex)[0]
val zipCode = address.split(cityZipCodeRegex)[1]

saveCityZipCode(city , zipCode)
```

**[⬆ về đầu trang](#mục-lục)**

### Tránh hại não người khác
Tường minh thì tốt hơn là ẩn.

**Không tốt:**
```kotlin
val l = listOf("Austin", "New York", "San Francisco")

l.forEach { 
    val li = it
        doStuff()
        doSomeOtherStuff()
        // ...
        // ...
        // ...
        // Khoan, `l` là cái gì vậy?
        dispatch(li)
 }
```

**Tốt:**

```kotlin
val locations = listOf("Austin", "New York", "San Francisco")

locations.forEach { 
    doStuff()
    doSomeOtherStuff()
    dispatch(it)
 }
```
**[⬆ về đầu trang](#mục-lục)**

### Đừng thêm những ngữ cảnh không cần thiết
Nếu tên của lớp hay đối tượng của bạn đã nói lên điều gì đó rồi, đừng lặp lại điều đó trong tên biến nữa.

**Không tốt:**
```kotlin
class Car {
  var carMake = "Honda"
  var carModel = "Accord"
  var carColor = "Blue"
}

fun paintCar(car: Car) {
  car.carColor = "Red"
}
```

**Tốt:**
```kotlin
class Car {
  var make = "Honda"
  var model = "Accord"
  var color = "Blue"
}

fun paintCar(car: Car) {
  car.color = "Red"
}
```
**[⬆ về đầu trang](#mục-lục)**
