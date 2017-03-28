# HDU Student ID Card Project

All the information you need to get access to HDU student ID cards. 

## License and Legal Stuff

This project is licensed under [WTFPL 2](http://www.wtfpl.net/about/).

THIS SOFTWARE IS PROVIDED BY THE AUTHOR AND CONTRIBUTORS ``AS IS'' AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE ARE DISCLAIMED. IN NO EVENT SHALL THE AUTHOR OR CONTRIBUTORS BE LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE POSSIBILITY OF SUCH DAMAGE.

## Types of Student Card

There are 2 types of student card. Everyone is given an initial student card when registering at school; this initial card is a Mifare 4K-compatible card. When you got a re-issued card from student center, the new card will be a Mifare 1K-compatible card. 

## Usage

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

Writing to a standard Mifare card: 

```shell
nfc-mfclassic w a edited_dump.mfd dumps/HDU-Mifare1k-empty.mfd f
```

Writing to a Chinese unlockable Mifare-compatible card:

```shell
nfc-mfclassic W a edited_dump.mfd
```

### Using MifareClassicTool

[MifareClassicTool](https://github.com/ikarus23/MifareClassicTool) is an Android application used to read and write Mifare cards. You need [a compatible device](https://github.com/ikarus23/MifareClassicTool/issues/20) to use it. 

#### Usage

Put `keys/*.keys` to your devices' `/sdcard/MifareClassicTool/key-files`, then when reading in app, select the corresponding keys file. (If you are not sure which card you are reading, tick both.)

## Thanks

All glory to the schoolmates who donated their card dumps to me.