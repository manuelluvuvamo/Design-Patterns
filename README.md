# Refactoring-Guru---Design-Patterns

## Padrões Criacionais

### Factory Method
O **Factory Method** é um padrão criacional de projeto que fornece uma interface para criar objetos em uma superclasse, mas permite que as subclasses alterem o tipo de objetos que serão criados.

![image](https://github.com/user-attachments/assets/d85c4fa7-24e3-47e9-a0d8-1d381c4013b6)

```php

  <?php
  
  namespace RefactoringGuru\FactoryMethod\Conceptual;
  
  abstract class Dialog
  {
      abstract public function createButton(): Button;
      
      public function render()
      {
          $okButton = $this->createButton();
          return $okButton->render();
      }
  }
  
  class WindowsDialog extends Dialog
  {
      public function createButton(): Button
      {
          return new WindowsButton();
      }
      
  }
  
  class WebDialog extends Dialog
  {
      public function createButton(): Button
      {
          return new WebButton();
      }
  }
  
  interface Button
  {
      public function render():string;
  }
  
  
  class WindowsButton implements Button
  {
      public function render():string
      {
          return "[A WINDOWS BUTTON]";
      }
  }
  
  
  class WebButton implements Button
  {
      public function render():string
      {
          return "[A HTML BUTTON]";
      }
  }
  
  
  
  function app(Dialog $dialog)
  {
      echo $dialog->render();
  }
  
  
  
  //Windows
  app(new WindowsDialog());
  echo "\n";
  //Web
  app(new WebDialog());
```


### Abstract Factory
O **Abstract Factory**  é um padrão de projeto criacional que permite que você produza famílias de objetos relacionados sem ter que especificar suas classes concretas.

![image](https://github.com/user-attachments/assets/1434e95e-c8cd-4ef8-9848-be51fdfc6659)

![image](https://github.com/user-attachments/assets/303c0726-04d0-404e-88fe-e93051cb9dcd)


```php
<?php
namespace RefactoringGuru\AbstractFactory\Conceptual;

interface GUIFactory {
    public function createButton(): Button;
    public function createCheckbox(): Checkbox;
}


interface Button {
    public function paint();
}

interface Checkbox {
    public function paint();
}


class WinFactory implements GUIFactory
{
    public function createButton(): Button
    {
        return new WinButton();
    }
    
    public function createCheckbox(): Checkbox
    {
        return new WinCheckbox();
    }
}


class MacFactory implements GUIFactory
{
    public function createButton(): Button
    {
        return new MacButton();
    }
    
    public function createCheckbox(): Checkbox
    {
        return new MacCheckbox();
    }
}


class WinButton implements Button
{
    public function paint()
    {
        return "Painting a Windows button";
    }
}


class MacButton implements Button
{
    public function paint()
    {
        return "Painting a Mac button";
    }
}

class WinCheckbox implements Checkbox
{
    public function paint()
    {
        return "Painting a Windows Checkbox";
    }
}


class MacCheckbox implements Checkbox
{
    public function paint()
    {
        return "Painting a Mac Checkbox";
    }
}



function app(GUIFactory $factory)
{
    $button = $factory->createButton();
    $checkbox = $factory->createCheckbox();

    echo $button->paint() . "\n";
    echo $checkbox->paint() . "\n";
}

/**
 * The client code can work with any concrete factory class.
 */
echo "Client: Testing client code with the first factory type:\n";
app(new WinFactory());

echo "\n";

echo "Client: Testing the same client code with the second factory type:\n";
app(new MacFactory());


```

### Builder
O **Builder** é um padrão de projeto criacional que permite a você construir objetos complexos passo a passo. O padrão permite que você produza diferentes tipos e representações de um objeto usando o mesmo código de construção.

![image](https://github.com/user-attachments/assets/e5eb42f7-39d5-4581-b9b1-857f3107b79e)
![image](https://github.com/user-attachments/assets/0429fe8b-eb3a-4482-a56d-9000a99f145a)


```php
<?php
namespace RefactoringGuru\Builder\Conceptual;

// Using the Builder pattern only makes sense when your products are
// complex and require extensive configuration. The following two
// following products are related, although they don't have
// a common interface.

class Car
{
  public $seats;
  public $engine;
  public $trip_computer;
  public $gps;

}


class Manual
{
  public $seats;
  public $engine;
  public $trip_compuer;
  public $gps;
}

// The builder interface specifies methods for creating the
// different parts of product objects.

interface Builder
{
  public function reset(): void;
  public function setSeats($number): void;
  public function setEngine($engine): void;
  public function setTripComputer($trip_computer = true): void;
  public function setGPS($gps = true): void;
}


// Concrete builder classes follow the builder

class CarBuilder implements Builder
{
  private Car $car;

  public function __construct()
  {
    $this->reset();
  }

  public function reset(): void
  {
    $this->car = new Car();
  }

  public function setSeats($number): void
  {
    $this->car->seats = $number;

  }

  public function setEngine($engine): void
  {
    $this->car->engine = $engine;

  }

  public function setTripComputer($trip_computer = true): void
  {
    $this->car->trip_computer = $trip_computer;

  }

  public function setGPS($gps = true): void
  {
    $this->car->gps = $gps;

  }

  // Concrete builders must provide their own
  // methods to retrieve the results. This is because
  // various types of builders can create
  // entirely different products that don't always follow the same
  // interface. Therefore, such methods cannot be
  // declared in the builder's interface (at least not in
  // a static type programming language).
  //
  // Generally, after returning the final result to the
  // client, a builder instance is expected to start
  // to produce another product. That's why it's
  // common to call the reset method at the end of the method body
  // `getProduct` method. However, this behavior is not
  // mandatory, and you can make your builder wait for
  // an explicit reset call from the client code
  // before getting rid of its previous result.

  public function getProduct(): Car
  {
    $product = $this->car;
    $this->reset();
    return $product;
  }
}


class CarManualBuilder implements Builder
{
  private Manual $manual;


  public function __construct()
  {
    $this->reset();
  }

  public function reset(): void
  {
    $this->manual = new Manual();
  }

  public function setSeats($number): void
  {
    $this->manual->seats = $number;

  }

  public function setEngine($engine): void
  {
    $this->manual->engine = $engine;

  }

  public function setTripComputer($trip_computer = true): void
  {
    $this->manual->trip_computer = $trip_computer;

  }

  public function setGPS($gps = true): void
  {
    $this->manual->gps = $gps;

  }

  // Concrete builders must provide their own
  // methods to retrieve the results. This is because
  // various types of builders can create
  // entirely different products that don't always follow the same
  // interface. Therefore, such methods cannot be
  // declared in the builder's interface (at least not in
  // a static type programming language).
  //
  // Generally, after returning the final result to the
  // client, a builder instance is expected to start
  // to produce another product. That's why it's
  // common to call the reset method at the end of the method body
  // `getProduct` method. However, this behavior is not
  // mandatory, and you can make your builder wait for
  // an explicit reset call from the client code
  // before getting rid of its previous result.

  public function getProduct(): Manual
  {
    $product = $this->manual;
    $this->reset();
    return $product;
  }
}

// The director is only responsible for carrying out the stages of
// construction steps in a particular sequence. This helps when
// producing products according to a specific order or
// configuration. Strictly speaking, the director class is optional, since the
// client can control the builders directly.

class Director
{
  // The director works with any builder instance that
  // the client code passes to it. In this way, the
  // client can change the final type of the newly
  // assembled. The director can build several variations
  // of the product using the same construction steps.

  /**
   * @var Builder
   */
  private $builder;

  public function setBuilder(Builder $builder): void
  {
    $this->builder = $builder;
  }

  public function constructSportsCar(): void
  {
    $this->builder->reset();
    $this->builder->setSeats(2);
    $this->builder->setEngine("Manual");
    $this->builder->setTripComputer(true);
    $this->builder->setGPS(true);
  }

  public function constructSUV(): void
  {
    // ... 
  }


}


// The client code creates a builder object, passes it to the
// director and then starts the build process. The final
// final result is retrieved from the builder object.

function app(Director $director)
{
  $builder = new CarBuilder();
  $director->setBuilder($builder);


  echo "Standard basic sport car:\n";
  $director->constructSportsCar();
  $car = $builder->getProduct();

  echo json_encode($car);

}


$director = new Director();
app($director);
```

**FONTES:** [Refactoring Guru](https://refactoring.guru/)

