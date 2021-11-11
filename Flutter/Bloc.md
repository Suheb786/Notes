# Bloc : Flutter Package

### [Tutorial Link](https://www.youtube.com/watch?v=THCkkQ-V1-8&t=5271s)

[TOC]



![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/JcnSFX(%202021-10-17%20)%20(%2012:41:16%20).png)

### Streams : 

Flow of data.



![Streems](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Streems202129.png)

![Screenshot 2021-10-16 at 9.31.47 AM](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%209.31.47%20AM202131202132.png)

**async* :**  Async generator function becouse  it generate async data so it initialised with *

**yield : ** It pushes data through stream.

------

![Screenshot 2021-10-16 at 9.46.20 AM](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%209.46.20%20AM202146.png)

### Cubit :

Cubit is a special kind of stream component which is based on some function which are called from UI and function that rebuild the UI by emitting different states on a stream.

![Screenshot 2021-10-16 at 9.49.14 AM](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%209.49.14%20AM202152.png)

![Screenshot 2021-10-16 at 9.56.29 AM](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%209.56.29%20AM202156.png)

![Screenshot 2021-10-16 at 9.58.26 AM](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%209.58.26%20AM202158(%202021-10-17%20)%20(%2022:14:13%20).png)

Cubit yield a stream we can also listen to it.

![Screenshot 2021-10-16 at 10.00.33 AM](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%2010.00.33%20AM202100.png)

Difference between `Cubit` and `Bloc` :

![Screenshot 2021-10-16 at 10.03.15 AM](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%2010.03.15%20AM202103.png)

------

### Managing State of a counter app by Cubit :

#### 1. BlocBuilder :

counter_cubit.dart :

```dart
import 'package:bloc/bloc.dart';

part 'counter_state.dart';

class CounterCubit extends Cubit<CounterState> {
  CounterCubit() : super(CounterState(counterValue: 0));
  void increment() => emit(CounterState(counterValue: state.counterValue + 1));
}
```

counter_state.dart : 

```dart
part of 'counter_cubit.dart';

class CounterState {
  int counterValue;
  CounterState({
    required this.counterValue,
  });
}
```

main.dart :

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

import 'cubit/counter_cubit.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);
  @override
  Widget build(BuildContext context) {
    return BlocProvider( // BlocProvider is used to Provide Data to it's subtree.
      create: (context) => CounterCubit(),
      child: const MaterialApp(
        title: 'Bloc Counter App',
        home: Home(),
      ),
    );
  }
}

class Home extends StatelessWidget {
  const Home({
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Bloc Counter App'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            // Wrap the widget with BlocBuilder where we need data.
            BlocBuilder<CounterCubit, CounterState>(
              builder: (context, state) {
                return Text(
                  state.counterValue.toString(),
                  style: Theme.of(context).textTheme.headline4,
                );
              },
            ),
          ],
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          BlocProvider.of<CounterCubit>(context).increment();
          // Seter to change data which is inside counter_cubit.dart file.
        },
        tooltip: 'Increment',
        child: const Icon(Icons.add),
      ),
    );
  }
}
```

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-16%20at%209.21.32%20PM202121(%202021-10-17%20)%20(%2022:35:45%20)(%202021-10-17%20)%20(%2022:37:13%20)(%202021-10-17%20)%20(%2022:38:17%20).png)

------

#### 2. BlocListener : 

- Show snack-bar using BlocListener.

counter_cubit.dart : 

```dart
import 'package:bloc/bloc.dart';

part 'counter_state.dart';

class CounterCubit extends Cubit<CounterState> {
  CounterCubit() : super(CounterState(counterValue: 0));
  void increment() => emit(
      CounterState(counterValue: state.counterValue + 1, wasIncremented: true));
  void decrement() => emit(CounterState(
      counterValue: state.counterValue - 1, wasIncremented: false));
}
```



counter_state.dart :

```dart
part of 'counter_cubit.dart';

class CounterState {
  int counterValue;
  bool? wasIncremented;
  CounterState({
    required this.counterValue,
    this.wasIncremented,
  });
}
```

main.dart :

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

import 'cubit/counter_cubit.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => CounterCubit(),
      child: const MaterialApp(
        title: 'Bloc Counter App',
        home: Home(),
      ),
    );
  }
}

class Home extends StatelessWidget {
  const Home({
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Bloc Counter App'),
      ),
      body: BlocListener<CounterCubit, CounterState>(
        listener: (context, state) {
          if (state.wasIncremented == true) {
            ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
              content: Text("Incremented"),
              duration: Duration(microseconds: 200),
            ));
          } else {
            ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
              content: Text("Decremented"),
              duration: Duration(microseconds: 200),
            ));
          }
        },
        child: Center(
          child: Column(
            mainAxisAlignment: MainAxisAlignment.center,
            children: <Widget>[
              const Text(
                'You have pushed the button this many times:',
              ),
              BlocBuilder<CounterCubit, CounterState>(
                builder: (context, state) {
                  return Text(
                    state.counterValue.toString(),
                    style: Theme.of(context).textTheme.headline4,
                  );
                },
              ),
              Row(
                mainAxisAlignment: MainAxisAlignment.spaceEvenly,
                children: [
                  FloatingActionButton(
                    onPressed: () {
                      BlocProvider.of<CounterCubit>(context).decrement();
                    },
                    tooltip: 'Increment',
                    child: const Icon(Icons.remove),
                  ),
                  FloatingActionButton(
                    onPressed: () {
                      BlocProvider.of<CounterCubit>(context).increment();
                    },
                    tooltip: 'Increment',
                    child: const Icon(Icons.add),
                  ),
                ],
              ),
            ],
          ),
        ),
      ),
    );
  }
}
```

------

#### 3. BlockConsumer :

BlockConsumer is a widget which combines both BlockBuilder and BlockListener into a single widget. 


main.dart :

```dart
import 'package:flutter/material.dart';
import 'package:flutter_bloc/flutter_bloc.dart';

import 'cubit/counter_cubit.dart';

void main() {
  runApp(const MyApp());
}

class MyApp extends StatelessWidget {
  const MyApp({Key? key}) : super(key: key);

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return BlocProvider(
      create: (context) => CounterCubit(),
      child: const MaterialApp(
        title: 'Bloc Counter App',
        home: Home(),
      ),
    );
  }
}

class Home extends StatelessWidget {
  const Home({
    Key? key,
  }) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Bloc Counter App'),
      ),
      body: Center(
        child: Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            const Text(
              'You have pushed the button this many times:',
            ),
            BlocConsumer<CounterCubit, CounterState>(
              listener: (context, state) {
                if (state.wasIncremented == true) {
                  ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
                    content: Text("Incremented"),
                    duration: Duration(microseconds: 200),
                  ));
                } else {
                  ScaffoldMessenger.of(context).showSnackBar(const SnackBar(
                    content: Text("Decremented"),
                    duration: Duration(microseconds: 200),
                  ));
                }
              },
              builder: (context, state) {
                return Text(
                  state.counterValue.toString(),
                  style: Theme.of(context).textTheme.headline4,
                );
              },
            ),
            Row(
              mainAxisAlignment: MainAxisAlignment.spaceEvenly,
              children: [
                FloatingActionButton(
                  onPressed: () {
                    BlocProvider.of<CounterCubit>(context).decrement();
                  },
                  tooltip: 'Increment',
                  child: const Icon(Icons.remove),
                ),
                FloatingActionButton(
                  onPressed: () {
                    BlocProvider.of<CounterCubit>(context).increment();
                  },
                  tooltip: 'Increment',
                  child: const Icon(Icons.add),
                ),
              ],
            ),
          ],
        ),
      ),
    );
  }
}
```

------

#### 4. MultiProviders : 

MultiProvider used to provide multiple `Cubit` or `Bloc`.

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-17%20at%2011.30.53%20AM(%202021-10-17%20)%20(%2012:27:06%20).png)

------

### Block Architecture : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Screenshot%202021-10-17%20at%2011.29.54%20AM(%202021-10-17%20)%20(%2012:33:10%20).png)

------

#### 1. DataLayer : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/1nc9Td(%202021-10-17%20)%20(%2012:48:24%20).png)

##### 1. The Models : 

> ![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/91rrye(%202021-10-17%20)%20(%2012:51:32%20).png)
>
> Data Models : 
>
> ![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/xCWXEw(%202021-10-17%20)%20(%2012:54:39%20).png)

##### 2. The Data Provider : 

> ![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/mKuivD(%202021-10-17%20)%20(%2013:01:48%20).png)

##### 3. The Repository : 

Here we fine-tune data got from API and assign it in respective Model class.

It's the part of DataLayer Bloc communicates with.

> ![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/PNVlEg(%202021-10-17%20)%20(%2013:16:39%20).png)
>
> ![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/oiIWWD(%202021-10-17%20)%20(%2013:08:48%20).png)

------

#### 2 .Business Logic Layer : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/ijEaI4(%202021-10-17%20)%20(%2013:29:22%20).png)

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/9rjiPV(%202021-10-17%20)%20(%2013:34:25%20).png)

Bloc can also communicate with other blocs :

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/Di0TrI(%202021-10-17%20)%20(%2013:36:12%20).png)

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/ITqK0y(%202021-10-17%20)%20(%2013:38:17%20).png)

#### 3. Presentation Layer : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/y6u2zc(%202021-10-17%20)%20(%2013:47:16%20).png)

------

Folders Arrengement in IDE ( Inside lib ) :

<img src="https://raw.githubusercontent.com/saffer4u/notes/master/uPic/QR2MI8(%202021-10-17%20)%20(%2013:50:51%20).png" style="zoom: 50%;" />

------

#### Basic Weather app architecture :

Request : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/t3YTkG(%202021-10-17%20)%20(%2013:57:33%20).png)

Response : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/hIeCPQ(%202021-10-17%20)%20(%2014:06:03%20).png)

------



### Testing : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/4oFGgv(%202021-10-17%20)%20(%2021:51:02%20).png)

For testing purpose we need to add two dependencies in `pubspec.yaml` :

1. `test:`
2. `bloc_test:`

For testing we need same hierarchy of folders and files inside the `test` folder as they are in `lib` folder, to differentiate between them we can put `_test` at the end of each file names.

Ex : `counter_cubit_test.dart`

```dart
import 'package:bloc_test/bloc_test.dart';
import 'package:flutter_bloc_concepts/cubit/counter_cubit.dart';
import 'package:test/test.dart';

void main() {
  group('CounterCubit', () { // Used to write multiple test in a single block.
    CounterCubit counterCubit;

    setUp(() {
      counterCubit = CounterCubit();
    }); // For Setup.

    tearDown(() {
      counterCubit.close();
    }); // Close cubit after test done.

    test('initial state of CounterCubit is CounterState(counterValue:0)', () {
      expect(counterCubit.state, CounterState(counterValue: 0));
    }); // Test for initial state of cubit.
    
    /*
    	But above test will not work because when we compare between two states the memory location of both states are compared not values of both states and because of the memory locations of both states are always different it give us test fail result.
    	
    	Soution : In order to compare values of states we need to import an external package called 
    	" equatable: " 
    	and we can extend our "CounterState" with equatable and create a getter for properties as mentioned in below code block.
    	=> Now if we run our test it'll work fine.
    */

    // 'blocTest' is used to test the functions inside the 'cubit' / 'bloc' class by which states are changeing.
    blocTest(
        'the CounterCubit should emit a CounterState(counterValue:1, wasIncremented:true) when the increment function is called',
        build: () => counterCubit,
        act: (cubit) => cubit.increment(),
        expect: [CounterState(counterValue: 1, wasIncremented: true)]);

    blocTest(
        'the CounterCubit should emit a CounterState(counterValue:-1, wasIncremented:false) when the decrement function is called',
        build: () => counterCubit,
        act: (cubit) => cubit.decrement(),
        expect: [CounterState(counterValue: -1, wasIncremented: false)]);
  });
}
```

`counter_state.dart` file inside lib/cubit folder.

```dart
part of 'counter_cubit.dart';

class CounterState extends Equatable {
  final int counterValue;
  final bool wasIncremented;

  CounterState({
    @required this.counterValue,
    this.wasIncremented,
  });

  @override
  List<Object> get props => [this.counterValue, this.wasIncremented];
}
```

------



### Flutter Routing : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/h1f0oS(%202021-10-18%20)%20(%2019:13:30%20).png)

`BlocProvider` can only provide instance of the `bloc` / `cubit` to the widgets of the same screen.

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/GicdpI(%202021-10-18%20)%20(%2019:16:57%20).png)

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/mspnoJ(%202021-10-18%20)%20(%2019:31:48%20).png)

There are three types of Routings : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/IAEhxt(%202021-10-18%20)%20(%2019:36:25%20).png)

#### 1. Anonymous Routing :

> ```dart
> MaterialButton(
>               color: color,
>               onPressed: () {
>                 Navigator.of(context).push(
>                   MaterialPageRoute(
>                     builder: (_) => const SecondScreen(
>                         title: "Second Screen", color: Colors.amber),
>                   ),
>                 );
>               },
>               child: const Text("Go to second screen"),
>             ),
> 
> // But the cubit or bloc must be provided to the second screen otherwise it'll through an error.
> ```
>
> In order to provide existing `bloc` or `cubit` to the second screen must be wrapped with the `BlocProvider.value()`
>
> ```dart
> MaterialButton(
>               color: color,
>               onPressed: () {
>                 Navigator.of(context).push(
>                   MaterialPageRoute(
>                     builder: (newContext) => BlocProvider.value(
>                       // There are two context, we should provide context of widget not builder 													context (newContext).
>                       value: BlocProvider.of<CounterCubit>(context),
>                       // This value should be of existing bloc otherwise it will make new instence
>                       child: const SecondScreen(
>                           title: "Second Screen", color: Colors.amber),
>                     ),
>                   ),
>                 );
>               },
>               child: const Text("Go to second screen"),
>             ),
> ```
>
> `BlocProvide.value()` is used to provide the value of existing created instance of bloc by `BlocProvide()` it doesn't initialise new instance. `BlocProvider.value()` is used pass the existing instance to Its subtree.



#### 2. Named Routing :

> ```dart
> class MyApp extends StatefulWidget {
>   const MyApp({Key? key}) : super(key: key);
> 
>   @override
>   State<MyApp> createState() => _MyAppState();
> }
> 
> class _MyAppState extends State<MyApp> {
>   final CounterCubit _counterCubit = CounterCubit();
>   // Initialize the cubit in order to provide same instance to each screen.
> 
>   @override
>   Widget build(BuildContext context) {
>     return MaterialApp(
>       debugShowCheckedModeBanner: false,
>       title: 'Bloc Counter App',
>       routes: {
>         '/': (context) => BlocProvider.value(
>               value: _counterCubit,
>               child: const HomeScreen(
>                 title: "Home Screen",
>                 color: Colors.greenAccent,
>               ),
>             ),
>         '/second': (context) => BlocProvider.value(
>               value: _counterCubit,
>               child: const SecondScreen(
>                 color: Colors.amber,
>                 title: "Second Screen",
>               ),
>             ),
>         '/third': (context) => BlocProvider.value(
>               value: _counterCubit,
>               child: const ThirdScreen(
>                 color: Colors.red,
>                 title: "Third Screen",
>               ),
>             ),
>       },
>       // home: const HomeScreen(
>       //   title: 'Bloc Counter App Home Screen',
>       //   color: Colors.blue,
>       // ),
>       // routes: along with home: will give an error.
>     );
>   }
>   
>   // Close the initialized cubit inside the dispose function.
>   @override
>   void dispose() {
>     _counterCubit.close();
>     super.dispose();
>   }
> }
> ```
>
> For Pushing new screen we should use `Navigator.of(context).pushNamed()` as mentioned below : 
>
> ```dart
> MaterialButton(
>               color: Colors.amber,
>               onPressed: () {
>                 Navigator.of(context).pushNamed(
>                  '/second'
>                 );
>               },
>               child: const Text("Go to second screen"),
>             ),
> ```



#### 3. Generated Routing : 

> ![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/VFioOt(%202021-10-30%20)%20(%2019:38:41%20).png)
>
> In generated routing we create a `route` folder inside the `presentation` folder, inside this route folder we create a dart file `app_router.dart` all the logic for routing will be defined inside the `AppRouter` class.
>
> `main.dart`  : 
>
> ```dart
> void main() {
>   runApp(const MyApp());
> }
> 
> class MyApp extends StatefulWidget {
>   const MyApp({Key? key}) : super(key: key);
>   @override
>   State<MyApp> createState() => _MyAppState();
> }
> 
> class _MyAppState extends State<MyApp> {
>   final AppRouter _appRouter = AppRouter();
>   // For generated routes first we need to initalize AppRouter Class.
>   @override
>   Widget build(BuildContext context) {
>     return MaterialApp(
>       debugShowCheckedModeBanner: false,
>       title: 'Bloc Counter App',
>       onGenerateRoute: _appRouter.onGenerateRoute,
>       // It's Importenet to pass onGenerateRoute function as an argument ( Without Parentheses) to onGenerateRoute parameter.
>     );
>   }
> 
>   @override
>   void dispose() {
>     _appRouter.dispose();
>     super.dispose();
>   }
> }
> ```
>
> `app_router.dart` : 
>
> ```dart
> import 'package:flutter/material.dart';
> import 'package:flutter_bloc/flutter_bloc.dart';
> 
> import '../../logic/cubit/counter_cubit.dart';
> import '../screens/home_screen.dart';
> import '../screens/second_screen.dart';
> import '../screens/third_screen.dart';
> 
> class AppRouter {
>   final CounterCubit _counterCubit = CounterCubit();
> 	// Initialize the Cubit.
>   Route? onGenerateRoute(RouteSettings routeSettings) {
>     // Create a function of return type Route (Could be nullable)
>     switch (routeSettings.name) {
>       case '/':
>         return MaterialPageRoute(
>           builder: (_) => BlocProvider.value(
>             value: _counterCubit,
>             child: const HomeScreen(
>               title: "Home Screen",
>               color: Colors.greenAccent,
>             ),
>           ),
>         );
>         break;
> 
>       case '/second':
>         return MaterialPageRoute(
>           builder: (_) => BlocProvider.value(
>             value: _counterCubit,
>             child: const SecondScreen(
>               color: Colors.amber,
>               title: "Second Screen",
>             ),
>           ),
>         );
>         break;
> 
>       case '/third':
>         return MaterialPageRoute(
>           builder: (_) => BlocProvider.value(
>             value: _counterCubit,
>             child: const ThirdScreen(
>               color: Colors.red,
>               title: "Third Screen",
>             ),
>           ),
>         );
>         break;
> 
>       default:
>         return null;
>     }
>   }
> 
>   void dispose() => _counterCubit.close();
> }
> ```



#### Global Access : 

> Provide a single instance of `bloc` or `cubit` to every screen of app.
>
> It can be done by wrapping `MaterialApp` widget with `BlocProvider`
>
> ![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/9Xi5Y7(%202021-10-30%20)%20(%2020:34:48%20).png)
>
> `main.dart`  : 
>
> ```dart
> import 'package:flutter/material.dart';
> import 'package:flutter_bloc/flutter_bloc.dart';
> import 'package:flutter_bloc_concept/logic/cubit/counter_cubit.dart';
> 
> import 'presentation/router/app_router.dart';
> 
> void main() {
>   runApp(MyApp());
> }
> 
> class MyApp extends StatelessWidget {
>   final AppRouter _appRouter = AppRouter();
>   MyApp({Key? key}) : super(key: key);
>   @override
>   Widget build(BuildContext context) {
>     return BlocProvider(
>       // Cubit is provided golobally, don't need to pass the bloc proveder value to every screen.
>       create: (context) => CounterCubit(),
>       child: MaterialApp(
>         debugShowCheckedModeBanner: false,
>         title: 'Bloc Counter App',
>         onGenerateRoute: _appRouter.onGenerateRoute,
>       ),
>     );
>   }
> }
> // Here we don't need to close the cubit because it closes automatically so we don't need to dispose it thus we can use StateLessWidget.
> ```
>
> `app_route.dart`  : 
>
> ```dart
> import 'package:flutter/material.dart';
> 
> import '../screens/home_screen.dart';
> import '../screens/second_screen.dart';
> import '../screens/third_screen.dart';
> 
> class AppRouter {
>   Route? onGenerateRoute(RouteSettings routeSettings) {
>     switch (routeSettings.name) {
>       case '/':
>         return MaterialPageRoute(
>           builder: (_) => const HomeScreen(
>             title: "Home Screen",
>             color: Colors.greenAccent,
>           ),
>         );
>         break;
> 
>       case '/second':
>         return MaterialPageRoute(
>           builder: (_) => const SecondScreen(
>             color: Colors.amber,
>             title: "Second Screen",
>           ),
>         );
>         break;
> 
>       case '/third':
>         return MaterialPageRoute(
>           builder: (_) => const ThirdScreen(
>             color: Colors.red,
>             title: "Third Screen",
>           ),
>         );
>         break;
> 
>       default:
>         return null;
>     }
>   }
> }
> ```

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/ZbcQ8M(%202021-10-30%20)%20(%2020:52:07%20).png)

------



### `Bloc` to `Bloc` Communication : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/8JzyAG(%202021-10-31%20)%20(%2019:34:22%20).png)

There are two possible scenarios we can tackle in order to achieve this : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/9JXSkV(%202021-10-31%20)%20(%2019:37:20%20).png)

#### 1. Stream Subscription Method : 

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/RLEzHJ(%202021-10-31%20)%20(%2019:41:39%20).png)

![](https://raw.githubusercontent.com/saffer4u/notes/master/uPic/H7Me2l(%202021-10-31%20)%20(%2020:23:34%20).png)
