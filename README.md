Sed (steam editor) intro

 * sed* به معنی* stream editor* یا همان ادیتور جریان داده است.

در ابتدا باید فایلی مناسب تمرین‌های این درس بسازیم:

user\$: vim quote.txt

متن زیر را داخل فایل قرار داده و ذخیره می‌کنیم.

There is only one thing that makes a dream impossible to achieve: the
fear of failure.

 - Paulo Coelho, The Alchemist

جهت درک شیوه کار *sed* ابتدا دستور زیر را اجرا کنید:

sed \'\'

سپس چیزی در ترمینال تایپ کنید. خواهید دید که سد عیناً چیزی را که تایپ
می‌کنید به شما باز می‌گرداند.

حالا دستور زیر را اجرا کنید

sed \'\' quote.txt

نتیجه آن عین متن داخل فایلی که ساختیم خواهد بود. علت آن این‌است که جهت
ادیت کردن متن با سد می‌بایست از دستورها و فلگ‌ها (*flag* ) و دیگر امکانات
آن استفاده کنیم.

Basic Syntax

سد به دو شیوه زیر قابل استفاده است

sed \[-n\] \[-e\] \'command(s)\' files

sed \[-n\] -f scriptfile files

در شیوه اول ما ادیت و الگوی آن را در خط فرمان به سد می‌دهیم و در شیوه دوم
فرامین سد را در یک فایل ذخیره کرده و به سد می‌دهیم.

فایلی با محتوای زیر بسازید و نام آن را *book.txt* بگذارید.

1\) A Storm of Swords, George R. R. Martin, 1216

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

سپس می‌خواهیم با استفاده از سد فقط خطوط مشخصی را پاک کنیم

user\$: sed -e \'1d\' -e \'2d\' -e \'5d\' books.txt

نتیجه به این شکل خواهد بود

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

6\) A Game of Thrones, George R. R. Martin, 864

همانطور که می‌بینید ما با استفاده از فلگ *e-* و مشخص کردن شماره خطوطی که
می‌خواهیم پاک شوند در کنار *d*، آن خطوط را پاک کردیم. فلگ *e-* به این
معناست که آنچه پس از آن می‌آید دستوراتی هستند که سد می‌بایست اجرا
*execute* کند.

حال می‌خواهیم همین کار را به شیوه دوم انجام دهیم.

user:\$ echo -e \"1d\\n2d\\n5d\" \> commands.txt

user:\$ cat commands.txt

وقتی فایل دستورها را *cat* کنیم خروجی زیر را می‌بینیم

1d

2d

5d

حالا به سد خواهیم گفت که دستورات را از این فایل بخواند:

user:\$ sed -f commands.txt books.txt

خروجی به شکل زیر خواهد بود

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

6\) A Game of Thrones,George R. R. Martin, 864

در این شیوه با استفاده از فلگ* f-* به معنی *file* سد را مجبور به خواندن
دستورات از فایل کردیم و سپس فایلی که میخواهیم ادیت کنیم را معین کردیم.

## Standard Options

آپشن‌های استاندارد سد به شرح زیر هستند:

-   -n: \--quiet, \--silent

-   اجرا در سکوت. خروجی در ترمینال نمایش داده نخواهد شد

    user:\$ sed -n \'\' quote.txt

-   -e

-    اجرای دستورات ادیت.

    بگذارید با استفاده از الگوی *p* هر خط را دوبار نمایش دهیم:

    user:\$ sed -e \'\' -e \'p\' quote.txt

نتیجه:

There is only one thing that makes a dream impossible to achieve: the
fear of failure.

There is only one thing that makes a dream impossible to achieve: the
fear of failure.

 - Paulo Coelho, The Alchemist

 - Paulo Coelho, The Alchemist

دلیل این رفتار ترکیب *'' e-* با* e p-* است. خواهید دید که p به تنهایی
مقادیر راخل فایل را نشان خواهد داد.

-   -f

-   خواندن فرامین از یک فایل

user:\$ echo \"p\" \> commands

user:\$ sed -n -f commands quote.txt

نتیجه:

There is only one thing that makes a dream impossible to achieve: the
fear of failure.

 - Paulo Coelho, The Alchemist

بیایید به فایل *books.txt* برگردیم و دستور زیر را اجرا کنیم

user:\$ sed 'p' books.txt

نتیجه آن به این شکل خواهد بود:

1\) A Storm of Swords, George R. R. Martin, 1216

1\) A Storm of Swords, George R. R. Martin, 1216

2\) The Two Towers, J. R. R. Tolkien, 352

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

6\) A Game of Thrones, George R. R. Martin, 864

همانطور که در بخش اول دیدیم، سد به تنهایی اطلاعات درون فایل را نمایش
می‌دهد. در بخش پیش نیز دیدم که فلگ* p* اطلاعات را پرینت می‌کند. در نتیجه
ترکیب این دو با هم باعث چنین نتیجه‌ای می‌شود. برای اینکه جلوی خروجی
استاندارد سد را بگیریم، کافیست از فلگ *n* استفاده کنیم.

User:\$ sed -n \'p\' books.txt

خروجی:

1\) A Storm of Swords, George R. R. Martin, 1216

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

بصورت پیش‌فرض سد روی کل فایل کار می‌کند. اما ما می‌توانیم آن را مجبور به
کار کردن روی فقط یک خط کنیم.

user:\$ sed -n \'3p\' books.txt

نتیجه دستور بالا، پرینت شدن تنها سومین خط از محتویات فایل است

3\) The Alchemist, Paulo Coelho, 197

همچنین می‌توانیم محدوده کار کردن سد را بصورت یک رنج مشخص کنیم

user:\$ sed -n \'2,5 p\' books.txt

نتیجه:

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

خطوط بین ۲ تا ۵ همانطور که می‌خواستیم پرینت شدند.

ما می‌توانیم از سمبل‌های مخصوصی نیز جهت مشخص کردن محدوده عملیات سد استفاده
کنیم.

علامت * \$ * به معنی آخرین خط می‌باشد. در نتیجه اگر بخواهیم فقط روی آخرین
خط کار کنیم می‌توانیم به این شکل عمل کنیم:

user:\$ sed -n \'\$ p\' books.txt

6\) A Game of Thrones, George R. R. Martin, 864

همچنین ‌می‌توانیم رنج را بصورت زیر نیز مشخص کنیم.

user:\$ sed -n \'2,+4 p\' books.txt

سمبل* \~* مقداری را گرفته و به همان خط می‌رود و سپس مقدار دوم را که رنج
پرش را معین کرده از آن‌جا اعمال می‌کند. یعنی:

50\~5

به خط ۵۰ رفته و سپس رنجی ۵ تایی را روی ادامه فایل اعمال می‌کند. در نتیجه
خطوط ۵۰ ، ۵۵ ، ۶۰ ،۶۵ و\... مورد پردازش قرار می‌گیرند.

-   دستور زیر چه کار می‌کند. شرح دهید

user:\$ sed -n \'3,\$ p\' books.txt

-   با استفاده از رنج به‌وسیله \~ یک بار فقط خطوط فرد و بار دیگر فقط خطوط
    زوج را نمایش دهید.
-   با استفاده از آنچه در این درس آموختید، دو فایل مشابه quote.txt و
    book.txt تهیه کنید و به ترتیب بخش‌ها عملیات‌های مشابهی را روی فایل‌ها
    انجام داده و اسکرین‌شات بگیرید. دقت کنید که کپی از تمرینات درس قابل
    قبول نیست.


# Pattern Range

در این فصل می‌خواهیم درباره‌ی الگوهای رنج براساس رشته (الگوی کلمات) صحبت
کنیم.

user\$: sed -n \'/Paulo/ p\' books.txt

دستور بالا تمام کتاب‌های پائولو کوئیلو را به ما نمایش می‌دهد.

3\) The Alchemist, Paulo Coelho, 197

5\) The Pilgrimage, Paulo Coelho, 288

این دستور به دنبال رشته‌ی *paulo* می‌گردد و تمامی خطوطی که شامل این کلمه
باشد را نمایش می‌دهد.

ما همچنین می‌توانیم رنج براساس الگو را با رنج براساس شماره خطوط که در درس
پیش دیدیم نیز ترکیب کنیم.

user\$: sed -n \'/Alchemist/, 5 p\' books.txt

دستور بالا از اولین باری که رشته‌ی *paulo* را پیدا کند تا پنجمین خط را
برای ما نمایش می‌دهد.

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

دستور زیر از اولین باری که رشته‌ی *The* را پیدا کند تا آخر فایل را نمایش
می‌دهد

user\$: sed -n \'/The/,\$ p\' books.txt

نتیجه

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

ما می‌توانیم الگوها را با علامت کما ، از هم تفکیک کرده و بیش از یک الگو
داشته باشیم

user\$: sed -n \'/Two/, /Pilgrimage/ p\' books.txt

دستور بالا هر آنچه بین الگوی اول و رشته‌ی *Pilgrimage* باشد را نمایش
می‌دهد.

نتیجه:

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

علاوه بر این، می‌توانیم از علامت + نیز استفاده کنیم.

user\$: sed -n \'/Two/, +4 p\' books.txt

نتیجه:

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

Basic Commands

## Delete Command

سد قابلیت‌های بسیاری برای تغیر متن دارد. نخست از قابلیت پاک کردن شروع
می‌کنیم. فرمول پاک کردن به شکل زیر است.

\[address1\[,address2\]\]d

آدرس‌ها همان الگوهایی هستند که در بخش پیشین دیدیم. به خاطر داشته باشید که
سد تنها محتوای نمایش داده شده را تغییر می‌دهد و اصل فایل را تغییر نمی‌ده
مگر آنکه جریان خروجی آن به یک فایل هدایت شود. در این مورد در دروس آینده
صحبت خواهیم کرد.

آدرس ۱ و ۲ محل شروع و پایان عملیات هستند. آدرس‌ها می‌توانند الگوی رشته‌ای
یا رنج خطی باشند

.

دستور زیر را اجرا کنید.

user\$: sed \'d\' books.txt

همانطور که دیدید این این دستور هیچ خروجی ندارد زیرا در صورت عدم مشخص
کردن آدرس، سد به صورت پیش‌فرض روی تمام خطوط کار می‌کند و در نتیجه تمامی
خطوط پاک می‌شوند.

حال می‌خواهیم سد را وادار به مار کردن بر خطوط مشخصی کنیم.

user\$: sed \'4d\' books.txt

نتیجه:

1\) A Storm of Swords, George R. R. Martin, 1216

2\) The Two Towers, J. R. R. Tolkien, 352

3\) The Alchemist, Paulo Coelho, 197

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

جهت استفاده از رنج خطی از الگوی زیر استفاده می‌کنیم.

user\$: sed \'2, 4 d\' books.txt

نتیجه:

1\) A Storm of Swords, George R. R. Martin, 1216

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

جهت استفاده از الگوی رشته‌ای:

user\$: sed \'/Paulo Coelho/d\' books.txt

نتیجه:

1\) A Storm of Swords, George R. R. Martin, 1216

2\) The Two Towers, J. R. R. Tolkien, 352

4\) The Fellowship of the Ring, J. R. R. Tolkien, 432

6\) A Game of Thrones, George R. R. Martin, 864

و همچنین می‌توانیم آدرس را با الگوهای متنی مشخصی کنیم.

user\$: sed \'/Storm/,/Fellowship/d\' books.txt

این دستور هر آنچه بین رشته‌ی Storm و Fellowship باشد را پاک می‌کند.

5\) The Pilgrimage, Paulo Coelho, 288

6\) A Game of Thrones, George R. R. Martin, 864

از آنجایی که قابلیت نمایش محتوای بین رشته‌ها و قابلیت پاک کردن از مهم‌ترین
موارد مصرف سد هستند، لازم است جهت ادامه دروس این مطالب به خوبی آموخته و
اصطلاحا ملکه ذهن شوند. در نتیجه تمرین‌های خود را به مثال‌های درس محدود
نکرده و تا می‌توانید این دو بخش را تمرین کنید.

می‌توانید خروجی دستورات مختلف یا فایل‌های مختلف را به سد بدهید و برای خود
معین کنید که کدام بخش از خروجی را می‌خواهید ببینید و به همین ترتیب به
تمرین ادامه دهید.

[]{#anchor}*YouTube:
*[*https://www.youtube.com/@wolandsmachine*](https://www.youtube.com/@wolandsmachine)

GitHub: https://github.com/wolandark

*Email: *[*contact-woland@proton.me*](mailto:contact-woland@proton.me)

Telegram: https://t.me/inlovedwithapenguin

*YouTube:
*[*https://www.youtube.com/@wolandsmachine*](https://www.youtube.com/@wolandsmachine)

GitHub: https://github.com/wolandark

*Email: *[*contact-woland@proton.me*](mailto:contact-woland@proton.me)

Telegram: https://t.me/inlovedwithapenguin
