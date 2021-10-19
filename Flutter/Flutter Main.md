

- [Flutter Basics :](#flutter-basics)
    - [IMPORT](#import)
    - [State Less Widget -](#state-less-widget-)
    - [SCAFFOLD -](#scaffold-)
    - [REFECTOR CODE -](#refector-code-)
    - [Flutter Properties :](#flutter-properties)
        - [Axis Alignments-](#axis-alignments-)
            - [mainAxisAlignment -](#mainaxisalignment-)

# Flutter Basics :

#### IMPORT

> always use relative path through "import fix extension" as --
> 
> ```dart
> import 'package:flutter/material.dart';
> import '../widgets/body.dart'; 
> import '../widgets/contact_button.dart';
> import '../widgets/row_button.dart';
> ```

* * *

#### State Less Widget -

> this widget is used for immutable state of screen.
> 
> ```dart
> class ExampleApp extends StatelessWidget {
>   const ExampleApp({ Key? key }) : super(key: key);
> 
>   @override
>   Widget build(BuildContext context) {
>     return Container(
>       
>     );
>   }
> }
> ```

* * *

#### SCAFFOLD -

> implements the basic material design visual layout structure.
> 
> ```dart
> home: Scaffold(
>         appBar: AppBar(
>           toolbarHeight: 70,
>           elevation: 0.0,
>           backgroundColor: Colors.white,
> ```

* * *

#### REFECTOR CODE -

1.  Extract Method steps :
    - select the widget
    - click on yellow light icon and Extract Widget

> We can copy the Extracted Widget and paste in on a new .dart file

* * *

## Flutter Properties :

#### Axis Alignments-

Alignment of axis changes according to Row or Column widgets.

1.  ##### mainAxisAlignment -
    
    > ```dart
    > Row(
    >       mainAxisAlignment: MainAxisAlignment.spaceBetween, 
    >       crossAxisAlignment: CrossAxisAlignment.stretch,
    >       children: []
    >       )
    > ```
    > 
    > here, widget will be aligned as horizontally.
    
2.  crossAxisAlignment -
    
    > ```dart
    > Row(
    >       mainAxisAlignment: MainAxisAlignment.spaceBetween, 
    >       crossAxisAlignment: CrossAxisAlignment.stretch,
    >       children: []
    >       )
    > ```
    > 
    > here, widget will be aligned as vertically.
    
    * * *
    
    [flutter](https://gist.github.com/Suheb786/82c4c5d5712f93738737a7fb0a3b0132)