![image](https://github.com/user-attachments/assets/2c2fa9dd-f42c-4525-8adc-1440bf19ded5)

---

# 📘 جزوه آموزش State در Jetpack Compose

## 🧾 پیش‌نیاز
- یه آشنایی اولیه با زبان Kotlin  
- بلد بودن راه انداختن یه پروژه با Jetpack Compose تو Android Studio

---

## 1. ✅ اصلاً State یعنی چی؟

تو Jetpack Compose، رابط کاربری یا همون UI بر اساس یه سری داده ساخته می‌شه. اگه این داده‌ها عوض بشن، خود کامپوز به‌طور خودکار دوباره UI رو می‌سازه. به این داده‌ها می‌گیم:

> 🎯 **state** = داده‌ای که وقتی تغییر کنه، UI هم آپدیت می‌شه.

---

## 2. 🔧 چطوری State بسازیم؟

تو Compose از دوتا ابزار مهم برای ساخت State استفاده می‌کنیم:

### 🟩 `mutableStateOf(value)`
یعنی: یه مقدار قابل تغییره که اگه تغییرش بدی، کامپوز خودش می‌فهمه و دوباره UI رو می‌سازه.

مثال:
```kotlin
val count = mutableStateOf(0)
count.value++  // این باعث می‌شه UI آپدیت بشه
```

ولی این کافـــی نیست...

---

### 🟨 `remember { ... }`
میاد مقدار `mutableStateOf` رو بین رسم‌های مجدد نگه می‌داره.  
چرا لازمه؟ چون کامپوز توی هر کلیک یا هر تغییر، ممکنه تابع رو دوباره اجرا کنه!  
اگه `remember` نذاریم، مقدارمون هر بار می‌پره!

### ✅ شکل درست استفاده:
```kotlin
var count by remember { mutableStateOf(0) }
```

---

## ⚠️ اگه remember یا mutableStateOf نباشه چی میشه؟

| ❌ اگه اینو حذف کنی... | 📉 چی میشه؟ |
|-------------------------|----------------------------|
| فقط `remember` رو حذف کنی | مقدار با هر کلیک می‌پره و دوباره مقدار اولیه می‌گیره. مثلاً همش صفر می‌مونه! |
| فقط `mutableStateOf` رو حذف کنی | تغییر می‌کنی، ولی UI هیچی نمی‌فهمه! چون State نیست. |
| هر دو رو حذف کنی | یه متغیر معمولی Kotlin داری. تغییرات کاملاً بی‌تأثیر توی UI. اصلاً کار نمی‌کنه. |

---

## 🧠 تشبیه ساده

فرض کن روی یه تخته می‌نویسی:

- `mutableStateOf` اون ماژیکیه که می‌نویسی.
- `remember` کاری می‌کنه که نوشته‌ت بعد از پاک شدن تخته، یادت بمونه و دوباره همون نوشته‌ها باشه.

---

## 🧪 چند تا مثال آسون برای فهم بهتر

### 👋 سلام دادن با state

```kotlin
@Composable
fun HelloState() {
    var name by remember { mutableStateOf("World") }

    Text(text = "Hello $name")
}
```

---

### 🔢 مثال: شمارنده ساده

```kotlin
@Composable
fun Counter() {
    var count by remember { mutableStateOf(0) }

    Column(
        horizontalAlignment = Alignment.CenterHorizontally,
        verticalArrangement = Arrangement.Center,
        modifier = Modifier.fillMaxSize()
    ) {
        Text(text = "Count: $count", fontSize = 24.sp)
        Button(onClick = { count++ }) {
            Text("افزایش")
        }
    }
}
```


---

## ✍️ چطوری با TextField کار کنیم؟

وقتی می‌خوای از کاربر **متنی بگیری** (مثلاً اسم، عدد، پیام و...)، از `TextField` یا `OutlinedTextField` استفاده می‌کنی.

### ✅ مراحل کلی استفاده از TextField:

1. ساختن یه state (با `remember` و `mutableStateOf`) برای نگه‌داری متن
2. نمایش TextField و وصل کردن state بهش
3. نمایش یا استفاده از متنی که کاربر وارد کرده

---

### 🔄 `onValueChange` چیه؟

وقتی کاربر یه چیزی توی TextField تایپ می‌کنه، تابع `onValueChange` اجرا می‌شه.

تو باید توی `onValueChange`، مقدار جدید رو بریزی توی state.  
یعنی خودت مسئول به‌روزرسانی هستی.

---

### ✳️ مثال ساده:

```kotlin
@Composable
fun SimpleInput() {
    var text by remember { mutableStateOf("") }

    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = text,  // این همون مقداریه که تو فیلد نشون داده می‌شه
            onValueChange = { newText ->
                text = newText  // اینجا داریم مقدار جدید رو ذخیره می‌کنیم
            },
            label = { Text("یه چیزی بنویس") }
        )

        Spacer(modifier = Modifier.height(8.dp))

        Text("تو نوشتی: $text")
    }
}
```

---

## 🧠 خلاصه:

| مورد | توضیح |
|------|-------|
| `value` | مقداری که قراره توی فیلد نمایش داده بشه |
| `onValueChange` | تابعی که هر بار کاربر چیزی تایپ کرد اجرا می‌شه |
| `mutableStateOf` | جایی که متن رو نگه‌می‌داریم (state) |
| `remember` | باعث می‌شه مقدارمون بین رسم‌های مجدد گم نشه |

---

اگه خواستی بخش مربوط به TextField رو پررنگ‌تر کنم یا همراه با شکل و دیاگرام بفرستم، کافیه بگی. ولی با همین توضیح، بچه‌ها قطعاً راحت درکش می‌کنن 😊

---

### ➕ مثال: جمع دو عدد

```kotlin
@Composable
fun AddTwoNumbers() {
    var input1 by remember { mutableStateOf("") }
    var input2 by remember { mutableStateOf("") }
    var result by remember { mutableStateOf(0) }

    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = input1,
            onValueChange = { input1 = it },
            label = { Text("عدد اول") }
        )

        OutlinedTextField(
            value = input2,
            onValueChange = { input2 = it },
            label = { Text("عدد دوم") }
        )

        Button(onClick = {
            val num1 = input1.toIntOrNull() ?: 0
            val num2 = input2.toIntOrNull() ?: 0
            result = num1 + num2
        }) {
            Text("جمع کن")
        }

        Text("نتیجه: $result")
    }
}
```

---

## 🎯 تمرین‌ها (با جواب)

### 🧪 تمرین ۱: شمارنده‌ای بساز که مقدار رو کم کنه

```kotlin
@Composable
fun DecreaseCounter() {
    var count by remember { mutableStateOf(0) }

    Column(horizontalAlignment = Alignment.CenterHorizontally) {
        Text("Count: $count")
        Button(onClick = { count-- }) {
            Text("کاهش")
        }
    }
}
```

---

### 🧪 تمرین ۲: با زدن دکمه، یه پیام نشون بده

```kotlin
@Composable
fun ShowMessage() {
    var showMessage by remember { mutableStateOf(false) }

    Column(horizontalAlignment = Alignment.CenterHorizontally) {
        Button(onClick = { showMessage = true }) {
            Text("نمایش پیام")
        }

        if (showMessage) {
            Text("سلام به همه!")
        }
    }
}
```

---

### 🧪 تمرین ۳: بگیر اسم طرف رو، سلام بده!

```kotlin
@Composable
fun WelcomeUser() {
    var name by remember { mutableStateOf("") }
    var submitted by remember { mutableStateOf(false) }

    Column(modifier = Modifier.padding(16.dp)) {
        OutlinedTextField(
            value = name,
            onValueChange = { name = it },
            label = { Text("نام شما") }
        )

        Button(onClick = { submitted = true }) {
            Text("ثبت")
        }

        if (submitted) {
            Text("سلام $name!")
        }
    }
}
```

---

## 🎓 جمع‌بندی 

- وقتی می‌خوای داده‌ای داشته باشی که با تغییرش، صفحه هم تغییر کنه → از **`mutableStateOf`** استفاده کن.
- وقتی می‌خوای اون مقدار بین رسم‌های مجدد گم نشه → از **`remember`** استفاده کن.
- ترکیب این دوتا میشه چیزی که توی کامپوز لازمه برای اینکه همه‌چی درست کار کنه.

---

