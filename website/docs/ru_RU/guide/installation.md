# Установка

## Проверьте, поддерживается ли ваше устройство

Скачайте приложение менеджера KernelSU с сайта [GitHub Releases](https://github.com/tiann/KernelSU/releases) или [Coolapk market](https://www.coolapk.com/apk/me.weishu.kernelsu) и установите его на устройство:

- Если приложение показывает `Unsupported`, это означает, что **Вы должны скомпилировать ядро самостоятельно**, KernelSU не будет и никогда не предоставит Вам загрузочный образ для прошивки.
- Если приложение показывает `Не установлено`, значит, ваши устройства официально поддерживаются KernelSU.

:::info
Для устройств, показывающих `Unsupported`, здесь находится [Unofficially-support-devices](unofficially-support-devices.md), вы можете скомпилировать ядро самостоятельно.
:::

## Резервное копирование стокового файла boot.img

Перед прошивкой необходимо создать резервную копию файла boot.img. Если возникнет ошибка загрузки, вы всегда сможете восстановить систему, перепрошив ее на заводскую загрузку с помощью fastboot.

::: warning
Прошивка может привести к потере данных, поэтому обязательно выполните этот шаг перед переходом к следующему шагу!!! При необходимости можно также создать резервную копию всех данных на телефоне.
:::

## Необходимые знания

### ADB и fastboot

По умолчанию в этом руководстве вы будете использовать инструменты ADB и fastboot, поэтому, если вы их не знаете, рекомендуем сначала воспользоваться поисковой системой, чтобы узнать о них.

### KMI

Kernel Module Interface (KMI), версии ядра с одинаковым KMI **совместимы** Это то, что в GKI означает "общий"; наоборот, если KMI отличается, то эти ядра несовместимы друг с другом, и прошивка образа ядра с другим KMI, чем у вашего устройства, может привести к bootloop.

В частности, для устройств GKI формат версии ядра должен быть следующим:

```txt
KernelRelease :=
Version.PatchLevel.SubLevel-AndroidRelease-KmiGeneration-suffix
w      .x         .y       -zzz           -k            -something
```

`w.x-zz-k` - версия KMI. Например, если версия ядра устройства `5.10.101-android12-9-g30979850fc20`, то его KMI - `5.10-android12-9`; теоретически оно может нормально загружаться с другими ядрами KMI.

::: tip
Обратите внимание, что SubLevel в версии ядра не является частью KMI! Это означает, что `5.10.101-android12-9-g30979850fc20` имеет тот же KMI, что и `5.10.137-android12-9-g30979850fc20`!
:::

### Версия ядра и версия Android

Обратите внимание: **Версия ядра и версия Android - это не обязательно одно и то же!**

Если вы обнаружили, что версия ядра `android12-5.10.101`, а версия системы Android - Android 13 или другая, не удивляйтесь, поскольку номер версии системы Android не обязательно совпадает с номером версии ядра Linux; Номер версии ядра Linux обычно соответствует версии системы Android, поставляемой с **устройством при его поставке**. При последующем обновлении системы Android версия ядра, как правило, не меняется. При необходимости прошивки **укажите версию ядра!!!**.

## Введение

Существует несколько способов установки KernelSU, каждый из которых подходит для разных сценариев, поэтому выбирайте их по своему усмотрению.

1. Установка с помощью пользовательского Recovery (например, TWRP)
2. Установка с помощью приложения для прошивки ядра, например, Franco Kernel Manager
3. Установка с помощью fastboot с использованием boot.img, предоставленного KernelSU
4. Восстановить boot.img вручную и установить его

## Установка с помощью пользовательского Recovery

Необходимые условия: На устройстве должен быть установлен пользовательский Recovery, например TWRP; если его нет или доступен только официальный Recovery, воспользуйтесь другим способом.

Шаг:

1. С [Release page](https://github.com/tiann/KernelSU/releases) KernelSU загрузите zip-пакет, начинающийся с AnyKernel3, который соответствует версии вашего телефона; например, версия ядра телефона - `android12-5.10. 66`, то следует скачать файл `AnyKernel3-android12-5.10.66_yyy-MM.zip` (где `yyyy` - год, а `MM` - месяц).
2. Перезагрузите телефон в TWRP.
3. С помощью adb поместите AnyKernel3-*.zip в /sdcard телефона и выберите установку в графическом интерфейсе TWRP; или вы можете напрямую `adb sideload AnyKernel-*.zip` для установки.

PS. Данный способ подходит для любой установки (не ограничиваясь начальной установкой или последующими обновлениями), если вы используете TWRP.

## Установка с помощью Kernel Flasher

Необходимые условия: Ваше устройство должно быть рутованным. Например, вы установили Magisk, чтобы получить root, или установили старую версию KernelSU и должны обновить ее до другой версии; если ваше устройство не укоренено, попробуйте другие методы.

Шаг:

1. Загрузите zip-архив AnyKernel3; инструкции по загрузке см. в разделе *Установка с помощью пользовательского Recovery*.
2. Откройте приложение для прошивки ядра и используйте предоставленный AnyKernel3 zip для прошивки.

Если вы раньше не использовали приложение для прошивки ядра, то наиболее популярными являются следующие.

1. [Kernel Flasher](https://github.com/capntrips/KernelFlasher/releases)
2. [Franco Kernel Manager](https://play.google.com/store/apps/details?id=com.franco.kernel)
3. [Ex Kernel Manager](https://play.google.com/store/apps/details?id=flar2.exkernelmanager)

PS. Этот способ более удобен при обновлении KernelSU и может быть выполнен без компьютера (сначала сделайте резервную копию!). .

## Установка с помощью boot.img, предоставленного KernelSU

Этот способ не требует наличия TWRP и root-прав на телефоне; он подходит для первой установки KernelSU.

### Найти подходящий boot.img

KernelSU предоставляет общий boot.img для устройств GKI, и его необходимо прошить в загрузочный раздел устройства.

Вы можете загрузить boot.img с [GitHub Release](https://github.com/tiann/KernelSU/releases), обратите внимание, что вы должны использовать правильную версию boot.img. Например, если на устройстве установлено ядро `android12-5.10.101`, то необходимо загрузить `android-5.10.101_yyy-MM.boot-<format>.img`. , необходимо загрузить `android-5.10.101_yyy-MM.boot-<format>.img`.(Соблюдайте соответствие KMI!).

Где `<format>` означает формат сжатия ядра в официальном boot.img, проверьте формат сжатия ядра в оригинальном boot.img, вы должны использовать правильный формат, например, `lz4`, `gz`; если вы используете неправильный формат сжатия, вы можете столкнуться с bootloop.

::: info
1. Вы можете использовать magiskboot для получения формата сжатия исходной загрузки; конечно, вы также можете спросить других, более опытных ребят с той же моделью, что и ваше устройство. Кроме того, формат сжатия ядра обычно не меняется, поэтому, если вы успешно загрузились с определенным форматом сжатия, вы можете попробовать этот формат позже.
2. Устройства Xiaomi обычно используют `gz` или **без сжатия**.
3. Для устройств Pixel следуйте приведенным ниже инструкциям.
:::

### прошить boot.img на устройство

Используйте `adb` для подключения устройства, затем выполните `adb reboot bootloader` для входа в режим fastboot, после чего используйте эту команду для прошивки KernelSU:

```sh
fastboot flash boot boot.img
```

::: info
Если устройство поддерживает `fastboot boot`, можно сначала использовать `fastboot boot boot.img`, чтобы попытаться использовать boot.img для загрузки системы. Если произойдет что-то непредвиденное, перезагрузите его снова для загрузки.
:::

### перезагрузка

После завершения прошивки необходимо перезагрузить устройство:

```sh
fastboot reboot
```

## Исправить boot.img вручную

Для некоторых устройств формат boot.img не так распространен, например, не `lz4`, `gz` или несжатый; наиболее типичным является Pixel, его boot.img имеет формат `lz4_legacy` со сжатием, ramdisk может быть `gz`, также может быть `lz4_legacy` со сжатием; в это время, если напрямую прошить boot.img, предоставляемый KernelSU, телефон может не загрузиться; в это время можно вручную исправить boot.img для достижения цели.

Как правило, существует два способа исправления:

1. [Android-Image-Kitchen](https://forum.xda-developers.com/t/tool-android-image-kitchen-unpack-repack-kernel-ramdisk-win-android-linux-mac.2073775/)
2. [magiskboot](https://github.com/topjohnwu/Magisk/releases)

Среди них Android-Image-Kitchen подходит для работы на ПК, а magiskboot нуждается в сотрудничестве мобильного телефона.

### Подготовка

1. Получите стоковый boot.img вашего телефона; его можно получить у производителя устройства, возможно, вам понадобится [payload-dumper-go](https://github.com/ssut/payload-dumper-go)
2. Загрузите zip-файл AnyKernel3, предоставленный KernelSU, который соответствует версии KMI вашего устройства (можно обратиться к разделу *Установка с помощью пользовательского Recovery*).
3. Распакуйте пакет AnyKernel3 и получите файл `Image`, который является файлом ядра KernelSU.

### Использование Android-Image-Kitchen

1. Загрузите программу Android-Image-Kitchen на свой компьютер.
2. Поместите файл boot.img в корневую папку Android-Image-Kitchen.
3. Выполните команду `./unpackimg.sh boot.img` в корневом каталоге Android-Image-Kitchen, в результате чего boot.img распакуется и появятся некоторые файлы.
4. Замените `boot.img-kernel` в каталоге `split_img` тем `образом`, который вы извлекли из AnyKernel3 (обратите внимание на изменение названия на boot.img-kernel).
5. Выполните команду `./repackimg.sh` в корневом каталоге 在 Android-Image-Kitchen; Вы получите файл с именем `image-new.img`; Прошейте этот boot.img с помощью fastboot (см. предыдущий раздел).

### Использование magiskboot

1. Загрузите последнюю версию Magisk с [Release Page](https://github.com/topjohnwu/Magisk/releases).
2. Переименуйте Magisk-*.apk в Magisk-vesion.zip и разархивируйте его.
3. Закачайте `Magisk-v25.2/lib/arm64-v8a/libmagiskboot.so` на устройство с помощью adb: `adb push Magisk-v25.2/lib/arm64-v8a/libmagiskboot.so /data/local/tmp/magiskboot`.
4. Установите на устройство стоковый boot.img и образ в AnyKernel3.
5. Войдите в оболочку adb и перейдите в каталог `/data/local/tmp/`, затем `chmod +x magiskboot`.
6. Войдите в adb shell и cd директории `/data/local/tmp/`, выполните команду `./magiskboot unpack boot.img` для распаковки `boot.img`, вы получите файл `kernel`, это и есть ваше стоковое ядро.
7. Замените `kernel` на `Image`: `mv -f Image kernel`.
8. Выполните команду `./magiskboot repack boot.img`, чтобы перепаковать boot img, и получите файл `new-boot.img`, прошейте его на устройство с помощью fastboot.

## Другие методы

На самом деле все эти способы установки имеют только одну основную идею - **заменить исходное ядро на ядро, предоставляемое KernelSU**; если это возможно, то установка возможна; например, возможны следующие способы.

1. Сначала установить Magisk, получить права root через Magisk, а затем с помощью kernel flasher прошить AnyKernel zip из KernelSU.
2. Использовать какой-либо инструментарий для прошивки на ПК, чтобы прошить ядро, предоставленное KernelSU.