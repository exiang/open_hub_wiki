## PHPUnit Test
Yii comes with quite a complete test solutions which including unit test with fixtures and selenium test for web browser like automated testing.

To run a unit test:
```
cd /protected/tests
../vendor/phpunit/phpunit/phpunit unit/JunkTest
```

## for Module
> Your module unit test should not be in the main application unit test folder
```
cd /protected/modules/YOUR_MODULE/tests
../../../vendor/phpunit/phpunit/phpunit unit/ModuleJunkTest
```