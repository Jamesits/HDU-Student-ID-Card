# HDU Student ID Card Project

All the information you need to extract information from HDU student ID cards. May apply to other _[hzsun](http://www.hzsun.com/)_ RFID cards; your mileage may vary.

杭州电子科技大学校园卡存储分析结果。可能也适用于其它[正元智慧](http://www.hzsun.com/)方案的 RFID 卡。

## License and Legal Stuff

This project is licensed under [WTFPL 2](http://www.wtfpl.net/about/).

THIS SOFTWARE IS PROVIDED BY THE AUTHOR AND CONTRIBUTORS ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## About Cards and Card Readers

### Types of Student Card

There are 2 types of student card. Everyone is given an initial student card when registering at school; this initial card is a Mifare 4K-compatible card. When you got a re-issued card from student center, the new card will be a Mifare 1K-compatible card. 

### Types of Card Readers

#### Payment Client

![Payment Client](http://www.hzsun.com/uploadfiles/images/20160309205944071.jpg)

[Payment Clients](http://www.hzsun.com/proDetail.aspx?PKID=114073) are typically used to pay use student card. They have a numeric keyboard and Ethernet connection. They can also be configured to:

* Undo the last transaction on this client (requires card presence and supervisor password)
* Charge money (This may be [another type of device](http://www.hzsun.com/proDetail.aspx?PKID=114071))
* Write specific data to specific card sector (programmable, will display "CArd" on screen)

And it can be configured to ask for payment password if paying a large amount of money in a single transaction. The authentication model is unclear. 

Counterfeit Mifare 1K clone cards will be reported as unrecognizable card. Maybe an private card authentication method is implemented somewhere.

#### Dormitory Gate Access Logger

Typically an infrared access logger which beeps when people pass, and have a card reader connecting to a Windows XP computer displaying card reader log. This logger may have racing condition, when you quickly put and take card in certain timing, the beeper won't beep or makes lower sound then usual.

The gate uses reflection infrared method to detect human; you may use some reflection board or infrared light to interfere it. The light source and detector is located on the 2 stands near the wall; the center stand provides 2 reflecation boards to each side.

#### Library Gate

![Gate machine product picture](http://www.hzsun.com/uploadfiles/2016-03-09/8fa5fa1d-5c75-4a3f-ab66-55df2e97ab91.jpg)

[A read-then-pass access control machine](http://www.hzsun.com/proDetail.aspx?PKID=114107) which displays student name on embedded LCD and also connects to an PC displaying log. In 2th floor.

#### Daily Running Check-in Machine

A metal box with a B/W LCD, camera, PM2.5 sensor and a card reader (maybe PN532). Uses wired power and wireless networking (frequency unclear, but the antennas are visible). The card reader is configured to read the first 8 bytes of sector 15 (no matter which card type) as ASCII (maybe also GBK, although usually there is only 8-digit numeric student ID) string then display it on the LCD. 

#### Other Readers in Student Service Center

* [Cash Charge Machine](http://www.hzsun.com/proDetail.aspx?PKID=114080) (yellow kiosk running Windows XP + Java application): charge with cash, change payment password, and query last 5 transactions.
* _To be done_

## Repository Content

### Keys

All HDU cards have 6 encrypted sectors and other sectors use the universal key `ffffffffffff`.

Keys (`.key` files) are in plain text format, hex representation, in the following sequence (one key per line): 

```
1 universal key for unencrypted sectors
6 A keys for encrypted sectors
6 B keys for encrypted sectors
```

These key files can be used directly in [MifareClassicTool](https://github.com/ikarus23/MifareClassicTool).

### Data Structs

Data structs (`.bt` files) are plain text files representint dump structure in C-like struct definitions to help analyze card dump files. They can be used directly as templates in [010 Editor](http://www.sweetscape.com/010editor/).

### Dumps

Empty dump files (`.mfd`) are provided with all data bytes nulled but keys kept.

## Dumping Instruction

### Using ACR122

[ACS ACR122](http://nfc-tools.org/index.php?title=ACR122) is a common-seen NFC reader. [Install Libnfc](http://nfc-tools.org/index.php?title=Libnfc) to get access to the tools we need.

In the following commands, use the empty dump file corresponding to your card type. 

#### Dumping (Reading)

```shell
nfc-mfclassic r a new_dump.mfd dumps/HDU-Mifare1k-empty.mfd f
```

#### Writing

Writing to a standard Mifare card (UID won't be changed): 

```shell
nfc-mfclassic w a edited_dump.mfd
```

Writing to a Chinese unlockable Mifare-compatible card:

```shell
nfc-mfclassic W a edited_dump.mfd
```

### Using MifareClassicTool

[MifareClassicTool](https://github.com/ikarus23/MifareClassicTool) is an Android application used to read and write Mifare cards. You need [a compatible device](https://github.com/ikarus23/MifareClassicTool/issues/20) to use it. 

#### Usage

Put `keys/*.keys` to your devices' `/sdcard/MifareClassicTool/key-files`, then when reading in app, select the corresponding keys file. (If you are not sure which card you are reading, tick both.)

### Using Proxmark3

_This section is to be done._

#### Dumping (Reading)

```shell
hf mf dump
```

#### Writing

CAUTION: Proxmark3 have [an issue preventing setting the correct key for encrypted sectors](https://github.com/Proxmark/proxmark3/issues/201).

## Thanks

All glory to the schoolmates who donated their card dumps to me.
