# Linux terminal

## How to convert from PNG to JPEG

To convert one file only

```
convert <FILE>.png <FILE>.jpeg
```

For batch operations

```
for i in *.png ; do convert "$i" "${i%.*}.jpeg" ; done
```
