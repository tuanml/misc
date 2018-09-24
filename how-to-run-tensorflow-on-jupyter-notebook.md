# How to run Tensorflow on Jupyter Notebook

If your Jupyter notebooks cannot import tensorflow, try these commands:

First, check available kernels `jupyter kernelspec list` and you will see an output like:

```sh
$ jupyter kernelspec list

Available kernels:
  python2    /usr/local/share/jupyter/kernels/python2
  python3    /usr/local/share/jupyter/kernels/python3
```

To edit the specs for `python3`, let's see which files are in its directory

```sh
$ ls /usr/local/share/jupyter/kernels/python3

kernel.json  logo-32x32.png  logo-64x64.png
```

So, `kernel.json` is the file we need to modify in order to make it works

```sh
$ vim /usr/local/share/jupyter/kernels/python3/kernel.json

{
 "argv": [
  "/usr/local/bin/python3", # <= assign proper path to your python executable
  "-m",
  "ipykernel_launcher",
  "-f",
  "{connection_file}"
 ],
 "display_name": "Python 3",
 "language": "python"
}

```

That's all you need! ;)
