# LEV

LEV is [libev](http://software.schmorp.de/pkg/libev.html) bindings for Common Lisp.

## Usage

```common-lisp
(ql:quickload :lev)

(cffi:defcallback stdin-cb :void ((evloop :pointer) (io :pointer) (revents :int))
  (declare (ignore revents))
  (lev:ev-io-stop evloop io))

(setf client-socket-fd 5)

(setf evloop (lev:ev-default-loop 0))
(sb-thread:make-thread
  (lambda ()
    (lev:ev-run evloop 0) ))

(setf stdin-watcher (cffi:foreign-alloc '(:struct lev:ev-io)))
(lev:ev-io-init stdin-watcher 'stdin-cb client-socket-fd lev:+EV-READ+)
(lev:ev-io-start evloop stdin-watcher)
        
(cffi:foreign-free stdin-watcher)
```

## Why not cl-ev?

We already have [cl-ev](https://github.com/sbryant/cl-ev) for libev bindings, however I found there's some problems in it.

* Wrapping with CLOS. It's bad for performance, obviously
* Incomplete API
* The author is inactive in Common Lisp world anymore

## See Also

* [libev's documentation](http://pod.tst.eu/http://cvs.schmorp.de/libev/ev.pod)

## Author

* Eitaro Fukamachi (e.arrows@gmail.com)

## Copyright

Copyright (c) 2014 Eitaro Fukamachi (e.arrows@gmail.com)

## License

Licensed under the BSD 2-Clause License.
