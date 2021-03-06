# Streaming

Some backends (see [backends summary](../backends/summary.md)) support streaming bodies. If that's the case, you can set a stream of the supported type as a request body using the `streamBody` method, instead of the usual `body` method.

```eval_rst
.. note::

 Here, streaming refers to (usually) non-blocking, asynchronous streams of data. To send data which is available as an ``InputStream``, or a file from local storage (which is available as a ``File`` or ``Path``), no special backend support is needed. See the documenttation on :doc:`setting the request body <body>`.
```

For example, using the [akka-http backend](../backends/akka.md), a request with a streaming body can be defined as follows:

```scala
import sttp.client._

import akka.stream.scaladsl.Source
import akka.util.ByteString

val chunks = "Streaming test".getBytes("utf-8").grouped(10).to(Iterable)
val source: Source[ByteString, Any] = Source.apply(chunks.toList.map(ByteString(_)))

basicRequest
  .streamBody(source)
  .post(uri"...")
```

```eval_rst
.. note:: A request with the body set as a stream can only be sent using a backend supporting exactly the given type of streams.
```

It's also possible to specify that the [response body should be a stream](../responses/body.md#streaming).
