# phasync

The `phasync` framework is a new coroutine library for PHP which works without any extensions. With phasync, you can use asynchronous IO operations while writing code the way you have always done it in PHP.

## Running Coroutines

Whenever you need to write code that uses coroutines, you simply use `phasync::run()`. You can use it in your existing codebase without changing anything else.

```php
class SomeController {

    public function someMethod(): string {
        // Create a context for using async code
        return phasync::run(function() {
            // Write your code here
            $client = new HttpClient();

            // Another coroutine just for fun
            $result = phasync::go(function() {
                for ($i = 0; $i < 10; $i++) {
                    echo "Counting: $i\n";
                    phasync::sleep(0.5);
                }
                return $i;
            });

            // These requests run in parallel
            $request1 = $client->get("http://example.com/");
            $request2 = $client->get("http://example2.com/");

            return [
              $request1,
              $request2,
              phasync::await($result)
            ];
        });
    }

}
```
