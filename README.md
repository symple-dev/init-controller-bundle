# Symple Initializable Controller

The **Symple Initializable Controller** Bundle permits initialization Symfony controllers before invocation of action methods.

Content:

- [Installation](#installation)
- [Configuration](#config)
- [Usage](#usage)
  - [Initializable Controller Interface](#usage-interface)
  - [Init Annotation](#usage-annotation)

<a name="installation"></a>
## Installation

The suggested installation method is via [composer](https://getcomposer.org):

``` sh
php composer.phar require symple-dev/init-controller-bundle:1.0.*
```

<a name="config"></a>
## Configuration

You can enable or disable **Symple Initializable Controller** in app/config.yml (default enabled):

``` yaml
init_controller:
    enabled: true
```

<a name="usage"></a>
## Usage

<a name="usage-interface"></a>
### Initializable Controller Interface

Implement the interface **Symple\Bundle\InitControllerBundle\Controller\InitControllerInterface** in your Controller.

``` php
<?php
namespace Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symple\Bundle\InitControllerBundle\Controller\InitControllerInterface;

class MyController extends Controller implements InitControllerInterface
{
    // ...
}
```
<a name="usage-annotation"></a>
### Init Annotation

The annotation **@Symple\Bundle\InitControllerBundle\Annotation\Init** tells that this method should be invoked before some action method (on _kernel.controller_ event).

Available configuration options:

- **priority** - integer value for invocation ordering (the greatest will be first), default 0
- **args** - array of method arguments (optional)

**Note:** that **@Init** annotation can be applied to public methods only.

``` php
<?php
namespace Controller;

use Symfony\Bundle\FrameworkBundle\Controller\Controller;
use Symple\Bundle\InitControllerBundle\Annotation\Init;
use Symple\Bundle\InitControllerBundle\Controller\InitControllerInterface;

class MyController extends Controller implements InitControllerInterface
{
    /**
     * @Init(priority = 200)
     */
    public function initialize1()
    {
        // do something
    }
    
    /**
     * @Init(priority = 100, args = {123, true})
     */
    public function initialize2($int, $bool = false)
    {
        // do something
    }
    
    public function indexAction()
    {
        // ...
    }
}
```

In this example following methods will be invoked before indexAction (or another action method):

1. `initialize1();`
2. `initialize2(123, true);`

You can apply multiple **@Init** annotations to the same method.
