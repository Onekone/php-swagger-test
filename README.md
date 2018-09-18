This is a fork. 
Please refer to the original [byjg/php-swagger-test](https://github.com/byjg/php-swagger-test) package on how to use it

## Using it as Functional Test cases

Swagger Test provide the class `SwaggerTestCase` for you extend and create a PHPUnit test case. The code will try to 
make a request to your API Method and check if the request parameters, status and object returned are OK. 

```php
<?php
/**
 * Create a TestCase inherited from SwaggerTestCase
 */
class MyTestCase extends \ByJG\Swagger\SwaggerTestCase
{
    protected $filePath = '/path/to/json/definition';
    
    /**
     * Test if the REST address /path/for/get/ID with the method GET returns what is
     * documented on the "swagger.json"
     */
    public function testGet()
    {
        $request = new \ByJG\Swagger\SwaggerRequester();
        $request
            ->withMethod('GET')
            ->withPath("/path/for/get/1");

        $this->assertRequest($request);
    }

    /**
     * Test if the REST address /path/for/get/NOTFOUND returns a status code 404.
     */
    public function testGetNotFound()
    {
        $request = new \ByJG\Swagger\SwaggerRequester();
        $request
            ->withMethod('GET')
            ->withPath("/path/for/get/NOTFOUND")
            ->assertResponseCode(404);

        $this->assertRequest($request);
    }

    /**
     * Test if the REST address /path/for/post/ID with the method POST  
     * and the request object ['name'=>'new name', 'field' => 'value'] will return an object
     * as is documented in the "swagger.json" file
     */
    public function testPost()
    {
        $request = new \ByJG\Swagger\SwaggerRequester();
        $request
            ->withMethod('POST')
            ->withPath("/path/for/post/2")
            ->withRequestBody(['name'=>'new name', 'field' => 'value']);

        $this->assertRequest($request);
    }

    /**
     * Test if the REST address /another/path/for/post/{id} with the method POST  
     * and the request object ['name'=>'new name', 'field' => 'value'] will return an object
     * as is documented in the "swagger.json" file
     */
    public function testPost2()
    {
        $request = new \ByJG\Swagger\SwaggerRequester();
        $request
            ->withMethod('POST')
            ->withPath("/path/for/post/3")
            ->withQuery(['id'=>10])
            ->withRequestBody(['name'=>'new name', 'field' => 'value']);

        $this->assertRequest($request);
    }

}
```