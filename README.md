Bazel breaks symlinks in `bazel-bin` when building another package:

```
┌─[james@james-desktop] - [~/temp/bazel-test] - [Wed Oct 12, 03:59]
└─[$]> bazel --blazerc=/dev/null build foo
INFO: Found 1 target...
Target //foo:foo up-to-date:
  bazel-bin/foo/foo
INFO: Elapsed time: 1.453s, Critical Path: 0.01s
┌─[james@james-desktop] - [~/temp/bazel-test] - [Wed Oct 12, 03:59]
└─[$]> ./bazel-bin/foo/foo
┌─[james@james-desktop] - [~/temp/bazel-test] - [Wed Oct 12, 03:59]
└─[$]> ll ./bazel-bin/foo/foo
lrwxrwxrwx 1 james james 101 Oct 12 15:59 ./bazel-bin/foo/foo -> /home/james/.cache/bazel/_bazel_james/769a1b51278bbd8965f1717a2c530e4b/execroot/bazel-test/foo/foo.sh
┌─[james@james-desktop] - [~/temp/bazel-test] - [Wed Oct 12, 03:59]
└─[$]> bazel --blazerc=/dev/null build baz
INFO: Found 1 target...
Target //baz:baz up-to-date:
  bazel-bin/baz/baz
INFO: Elapsed time: 0.231s, Critical Path: 0.01s
┌─[james@james-desktop] - [~/temp/bazel-test] - [Wed Oct 12, 04:00]
└─[$]> ./bazel-bin/foo/foo
zsh: no such file or directory: ./bazel-bin/foo/foo
┌─[james@james-desktop] - [~/temp/bazel-test] - [Wed Oct 12, 04:00]
└─[$]> ll ./bazel-bin/foo/foo
lrwxrwxrwx 1 james james 101 Oct 12 15:59 ./bazel-bin/foo/foo -> /home/james/.cache/bazel/_bazel_james/769a1b51278bbd8965f1717a2c530e4b/execroot/bazel-test/foo/foo.sh
┌─[james@james-desktop] - [~/temp/bazel-test] - [Wed Oct 12, 04:01]
└─[$]> ll /home/james/.cache/bazel/_bazel_james/769a1b51278bbd8965f1717a2c530e4b/execroot/bazel-test/
total 60
lrwxrwxrwx 1 james james    31 Oct 12 16:00 baz -> /home/james/temp/bazel-test/baz
drwxrwxr-x 3 james james  4096 Oct 12 16:00 bazel-out
drwxrwxr-x 2 james james 49152 Oct 12 16:00 _bin
```
