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


**FONTES:** [Refactoring Guru](https://refactoring.guru/)

