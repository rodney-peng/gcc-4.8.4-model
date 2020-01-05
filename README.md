# C Modeling examples:

Demonstrates:

1.  concept of model, specification and implementation
2.  abstraction of module and procedure


Files:


     project/
         main.model
         main.spec
         mylib/
              mylib.impl
     stdlib.impl




main.model:

```
// no need to include any header file


int main() // () designates a function
{
    /* A module and procedure are only abstractions;
        they are not bound to any implementation until a specification is given
    */
    module.test {}; // {} designates an abstraction procedure


    return 0;
}
```



main.spec:


case 1: uses stdlib

```
/* <> looks file upward starting with ../ of the current spec file */
#define module    <stdlib.impl>
```

case 2: specified here

```
#define stdlib    <stdlib.impl>


module.test {}
{
    stdlib.print { msg="test in main.spec\n" };
    module.test {};     // recursion is invalid
}
```

case 3: inherited

```
#define module    <stdlib.impl>


module.test {}
{
    module.print { msg="test in main.spec\n" };
    parent {};
}
```

case 4: double inheritances

```
#define module    <stdlib.impl>


module.test {}
{
    module.print { msg="test1 in main.spec\n" };
    parent {};
}


module.test {}
{
    parent {};
    module.print { msg="test2 in main.spec\n" };
}
```

case 5: re-specified

```
#define module    <stdlib.impl>


/* "" looks file only in the folder (./) of the current spec file */
#define mylib     "mylib.impl"
#define module.test     mylib.proc
```

case 6: re-specified by inheritance

```
#define module    <stdlib.impl>
#define mylib     "mylib.impl"


module.test {}
{
     mylib.proc {};
}
```

case 7: re-specified and inherited

```
#define stdlib    <stdlib.impl>
#define module     stdlib


#define mylib     "mylib.impl"
#define module.test     mylib.proc


module.test {}
{
    module.print { msg="test in main.spec\n" };
    parent {};
    stdlib.test {};
}
```



stdlib.impl:

```
#include <stdio.h>


void test {}
{
    printf( "test in stdlib\n" );
}


void print { msg }
{
     printf( msg );
}
```



mylib.impl:

```
#include <stdio.h>


void proc {}
{
    printf( "proc in mylib\n" );
}
```
