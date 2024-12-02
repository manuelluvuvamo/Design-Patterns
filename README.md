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


**FONTES:** [Refactoring Guru](https://refactoring.guru/)

