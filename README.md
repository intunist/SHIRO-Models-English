# SHIRO-Models-English
English Multi-Speaker Models for the SHIRO Phoneme Alignment Toolkit.

Intunists's English models for [SHIRO](https://github.com/Sleepwalking/SHIRO).
The speech models are trained on ~400 hours of multi-speaker english speech.
The singing models are trained on top of the speech models with ~3 hours of multi-speaker singing.

These models use the Intunist English phoneme set, based on Arpabet.

Speech models are available in two versions, "type a" and "type b". Choose the alignment model that works best for you particular dataset.
Speech models will work well for most general voice types, if you encounter a voice that does not work well please feel free to submit it for training.

Singing model (unavailable) is a generic model for any voice type, unlike japanese separate models will not be provided for different vocal types.

# Quick start guide
## Easy SHIRO install (Ubuntu, tested in WSL2)
(this will put shiro in the current folder, copy as one line.)
```
wget https://raw.githubusercontent.com/intunist/SHIRO-Models-English/main/install-shiro-ubuntu.sh && chmod +x install-shiro-ubuntu.sh && ./install-shiro-ubuntu.sh

```

## How to use:
Build SHIRO using the directions in the SHIRO readme or above "Easy Install".

Perform feature extraction:
```
lua shiro-fextr.lua /path/to/your/index.csv -d /path/to/your/dataset -x ./extractors/extractor-xxcc-mfcc12-da-16k -r 16000
```

Create a placeholder alignment:
```
lua shiro-mkseg.lua /path/to/your/index.csv -m /path/to/intunist-en1_phonemap.json -d /path/to/your/dataset -e .param -n 36 -L pau -R pau > /path/to/your/dataset/unaligned.json
```

Perform the first alignment:
```
./shiro-align -m /path/to/intunist-en1_MODEL_NAME_TYPE.hsmm -s /path/to/your/dataset/unaligned.json -g > /path/to/your/dataset/initial.json
```

Perform the final alignment:
```
./shiro-align -m /path/to/intunist-en1_MODEL_NAME_TYPE.hsmm -s /path/to/your/dataset/initial.json -p 60 -d 80 > /path/to/your/dataset/refined.json
```

Convert final alignment to audacity labels:
```
lua shiro-seg2lab.lua /path/to/your/dataset/refined.json -t 0.005
```
This will create a text file for each audio file that can be viewed in Audacity.

If you want to convert these Audacity labels to the HTK LAB format (which is required for NNSVS/ENUNU) then you can use [Audacitylabel2Lab](https://github.com/oatsu-gh/oto2lab/tree/master/tool/ust2shiroindex) to convert them.
