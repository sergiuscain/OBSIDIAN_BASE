- Adb pull - Копирует файл из эмулятора.
``` cmd
adb pull /data/user/0/com.SergeyKorolev.SigmaWord/files/SigmaWordDb.db C:\Users\sergi\Downloads\
```
- Adb Root - Выдает рут права.
```cmd
adb root
```
- Adb shell wm size  - Получение физического разрешения экрана.
```cmd
adb shell wm size
```
- Получение плотности 
```cmd
Adb shell wm density
```
- Списка подключенных устройств.
```cmd
adb devices
```
- Устанавливаем разрешение 1080x1920 (9:16).
```cmd
adb shell wm size 1080x1920
```
- Сброс до физического разрешения.
```cmd
adb shell wm size reset
```
- Устанавливаем плотность пикселей.
```cmd
adb shell wm density 480
```
- Сброс плотности пикселей.
```cmd
adb shell wm density reset
```
- Перегрузка устройства.
```cmd
adb reboot
```