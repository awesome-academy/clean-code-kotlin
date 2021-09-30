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

**[⬆ Về đầu trang](#mục-lục)**

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

**[⬆ Về đầu trang](#mục-lục)**

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

**[⬆ Về đầu trang](#mục-lục)**

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

**[⬆ Về đầu trang](#mục-lục)**

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
**[⬆ Về đầu trang](#mục-lục)**

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
**[⬆ Về đầu trang](#mục-lục)**

## Hàm
### Đối số của hàm (lý tưởng là ít hơn hoặc bằng 2)
Giới hạn số lượng param của hàm là một điều cực kì quan trọng bởi vì nó làm cho
hàm của bạn trở nên dễ test hơn. Trường hợp có nhiều hơn 3 params có thể dẫn
đến việc bạn phải test hàng tấn test case khác nhau với những đối số riêng biệt.

1 hoặc 2 đối số là trường hợp lý tưởng, còn trường hợp 3 đối số thì nên tránh
nếu có thể. Những trường hợp khác (từ 3 params trở lên) thì nên được gộp lại.
Thông thường nếu có nhiều hơn 2 đối số thì hàm của bạn đang cố thực hiện quá
nhiều việc rồi đấy. Trong trường hợp ngược lại, phần lớn thời gian một đối
tượng cấp cao sẽ là đủ để làm đối số.

**[⬆ Về đầu trang](#mục-lục)**


### Hàm chỉ nên giải quyết một vấn đề
Đây là quy định quan trọng nhất của kỹ thuật phần mềm. Khi một hàm thực hiện
nhiều hơn 1 việc, chúng sẽ trở nên khó khăn hơn để viết code, test, và suy luận.
Khi bạn có thể tách biệt một hàm để chỉ thực hiện một hành động, thì sẽ dễ dàng
hơn để tái cấu trúc và code của bạn sẽ dễ đọc hơn nhiều. Nếu bạn chỉ cần làm theo
hướng dẫn này thôi mà không cần làm gì khác thì bạn cũng đã giỏi hơn nhiều
developer khác rồi.

**Không tốt:**
```kotlin
fun emailClients(clients: List<Client>) {
    clients.forEach { 
        val clientRecord = repository.findOne(it.getId())
        if(clientRecord.isActive()) {
            email(it)
        }
     }
}
```

**Tốt:**
```kotlin
fun emailClients(clients: List<Client>) {
    clients.forEach { 
        if(isActiveClient(it)) {
            email(it)
        }
     }
}

fun isActiveClient(client: Client) = repository.findOne(client.getId()).isActive()
```

**[⬆ Về đầu trang](#mục-lục)**

### Tên hàm phải nói ra được những gì chúng làm

**Không tốt:**
```kotlin
private fun addToDate(date: Date , month: Integer){
    //..
}

val date = Date()

// Khó để biết được hàm này thêm gì thông qua tên hàm.
addToDate(date, 1)
```
**Tốt:**
```kotlin
private fun addMonthToDate(date: Date , month: Integer){
    //..
}

val date = Date()
addMonthToDate(date , 1)
```

**[⬆ Về đầu trang](#mục-lục)**

### Xóa code trùng lặp
Tuyệt đối tránh những dòng code trùng lặp. Code trùng lặp thì không tốt bởi vì
nếu bạn cần thay đổi cùng một logic, bạn phải sửa ở nhiều hơn một nơi.

Hãy tưởng tượng nếu bạn điều hành một nhà hàng và bạn theo dõi hàng tồn kho:
bao gồm cà chua, hành tây, tỏi, gia vị, vv.... Nếu bạn có nhiều danh sách
quản lý, thì tất cả chúng phải được thay đổi khi bạn phục vụ một món ăn có
chứa cà chua. Nếu bạn chỉ có 1 danh sách, thì việc cập nhật ở một nơi thôi.

Thông thường, bạn có những dòng code lặp lại bởi vì bạn có 2 hay nhiều hơn
những thứ chỉ khác nhau chút ít, mà chia sẻ nhiều thứ chung, nhưng sự khác
nhau của chúng buộc bạn phải có 2 hay nhiều hàm riêng biệt để làm nhiều điều
tương tự nhau. Xóa đi những dòng code trùng có nghĩa là tạo ra một abstraction
có thể xử lý tập những điểm khác biệt này chỉ với một hàm/module hay class.

Có được một abstraction đúng thì rất quan trọng, đó là lý do tại sao bạn nên
tuân thủ các nguyên tắc SOLID được đặt ra trong phần *Lớp*. Những abstraction
không tốt có thể còn tệ hơn cả những dòng code bị trùng lặp, vì thế hãy cẩn
thận! Nếu bạn có thể tạo ra một abstraction tốt, hãy làm nó! Đừng lặp lại chính
mình, nếu bạn không muốn đi cập nhật nhiều nơi bất cứ khi nào bạn muốn thay đổi
một thứ gì đó.

**[⬆ Về đầu trang](#mục-lục)**

### Tránh những ảnh hưởng phụ (Side Effect)
Một hàm tạo ra ảnh hưởng phụ nếu nó làm bất kì điều gì khác hơn là nhận một giá
trị đầu vào và trả về một hoặc nhiều giá trị. Ảnh hưởng phụ có thể là ghi một
file, thay đổi vài biến toàn cục, hoặc vô tình đưa tất cả tiền của bạn cho một
người lạ.

Bây giờ, cũng có khi bạn cần ảnh hưởng phụ trong một chương trình. Giống như ví dụ
trước, bạn cần ghi một file. Những gì bạn cần làm là tập trung vào nơi bạn sẽ làm
nó. Đừng viết hàm và lớp riêng biệt để tạo ra một file cụ thể. Hãy có một service
để viết nó. Một và chỉ một.

Điểm chính là để tránh những lỗi chung như chia sẻ trạng thái giữa những đối tượng
mà không có bất kì cấu trúc nào, sử dụng các kiểu dữ liệu có thể thay đổi được mà
có thể được ghi bởi bất cứ thứ gì, và không tập trung nơi có thể xảy ra các ảnh hưởng
phụ. Nếu bạn có thể làm điều đó, bạn sẽ hạnh phúc hơn so với phần lớn các lập trình
viên khác đấy.

**Không tốt:**
```kotlin
// Biến toàn cục được tham chiếu bởi hàm dưới đây.
// Nếu chúng ta có một hàm khác sử dụng name, nó sẽ trả về kết quả sai
val name = "Ryan McDermott"

fun splitIntoFirstAndLastName() {
  name = name.split(" ").first()
}

splitIntoFirstAndLastName()

Log.d("function" , "Name: $name") // Name: Ryan
```

**Tốt:**
```kotlin
fun splitIntoFirstAndLastName(name: String) = name.split(" ").first()

val name = "Ryan McDermott"
val newName = splitIntoFirstAndLastName(name)

Log.d("function" , "Name: $name") // Name: Ryan McDermott
Log.d("function" , "Name: $newName") // Name: Ryan
```

**[⬆ Về đầu trang](#mục-lục)**

### Đóng gói các điều kiện

**Không tốt:**
```kotlin
if ("fetching" == fsm.getState() && listNode.isEmpty()) {
  // ...
}
```

**Tốt:**
```kotlin
fun shouldShowProgressBar(fsm: Any, listNode: String) = 
    "fetching" == fsm.getState() && listNode.isEmpty()

if (shouldShowProgressBar(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ Về đầu trang](#mục-lục)**

### Trách những điều kiện phủ định

**Không tốt:**
```kotlin
fun isDOMNodeNotPresent(node: Any): Boolean {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**Tốt:**
```kotlin
fun isDOMNodePresent(node: Any): Boolean {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```
**[⬆ Về đầu trang](#mục-lục)**

### Tránh điều kiện
Đây dường như là một việc bất khả thi. Khi nghe điều này đầu tiên, hầu hết mọi
người đều nói, "Làm sao tôi cần phải làm gì mà không có mệnh đề `if`?"
Câu trả lời là bạn có thể sử dụng tính đa hình để đạt được công việc tương tự
trong rất nhiều trường hợp. Câu hỏi thứ hai thường là "Đó là điều tốt nhưng tại
sao tôi lại muốn làm điều đó?" Câu trả lời là khái niệm mà ta đã học ở phần
trước: một hàm chỉ nên thực hiện một việc. Khi bạn có nhiều lớp và hàm mà có
nhiều mệnh đề `if`, bạn đang cho người dùng của bạn biết rằng hàm của bạn đang
làm nhiều hơn một việc. Hãy nhớ, chỉ làm một công việc thôi.

**Không tốt:**
```kotlin
class Airplane {
  // ...
  fun getCruisingAltitude(airplane: Airplane) =
    when(type) {
         "777" -> airPlane.getMaxAltitude() - airPlane.getPassengerCount()
         "Air Force One" -> airPlane.getMaxAltitude()
         "Cessna" -> airPlane.getMaxAltitude() - airPlane.getFuelExpenditure()
    }
}
```

**Tốt:**
```Kotlin
class Airplane {
  // ...
}

class Boeing777: Airplane {
  // ...
  override fun getCruisingAltitude() = getMaxAltitude() - getPassengerCount()
}

class AirForceOne: Airplane {
  // ...
  override fun getCruisingAltitude() = getMaxAltitude()
}

class Cessna: Airplane {
  // ...
  override fun getCruisingAltitude() = getMaxAltitude() - getFuelExpenditure()
}
```
**[⬆ Về đầu trang](#mục-lục)**

### Xóa code chết (dead code)
Dead code cũng tệ như code trùng lặp. Không có lý do gì để giữ chúng lại trong
codebase của bạn. Nếu nó không được gọi nữa, hãy bỏ nó đi! Nó vẫn sẽ nằm trong
lịch sử phiên bản của bạn nếu bạn vẫn cần nó.

**[⬆ Về đầu trang](#mục-lục)**