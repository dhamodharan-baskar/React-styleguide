# React-styleguide

Identifiers

Identifiers must use only ASCII letters, digits, underscores (for constants and structured test method names).

Style

Category

UpperCamelCase

class / interface / type / enum / decorator / type parameters

lowerCamelCase

variable / parameter / function / method / property / module alias


CONSTANT_CASE

global constant values, including enum values




#ident

	

private identifiers are never used.

Abbreviations: Treat abbreviations like acronyms in names as whole words, i.e. use loadHttpUrl, not loadHTTPURL, unless required by a platform name (e.g. XMLHttpRequest).
Dollar sign: Identifiers should not generally use $, except when aligning with naming conventions for third party frameworks.
Type parameters: Type parameters, like in Array<T>, may use a single upper case character (T) or UpperCamelCase.
_ prefix/suffix: Identifiers must not use _ as a prefix or suffix. Exception: When a certain variable is not getting used in the function definition.

Constants: CONSTANT_CASE indicates that a value is intended to not be changed, and may be used for values that can technically be modified (i.e. values that are not deeply frozen) to indicate to users that they must not be modified.

const UNIT_SUFFIXES = {
  'milliseconds': 'ms',
  'seconds': 's',
};

// Even though per the rules of JavaScript UNIT_SUFFIXES is
// mutable, the uppercase shows users to not modify it.
// A constant can also be a static readonly property of a class.

class Foo {
  private static readonly MY_SPECIAL_NUMBER = 5;
  ...
Use const

Use const for variable declarations as much as possible. Var should not be used at all and let should only be used when there is no way to achieve the given functionality without mutating the variable.

Non-ASCII characters

For non-ASCII characters, either the actual Unicode character (e.g. ∞) or the equivalent hex or Unicode escape (e.g. \u221e) is used

/* Best: perfectly clear even without a comment. */
const units = 'μs';

/* Allowed: but unnecessary as μ is a printable character. */
const units = '\u03bcs'; // 'μs'




/* Poor: the reader has no idea what character this is. */
const units = '\u03bcs';
Importing Files

All the files that are getting imported should use a fully qualified path from the src directory, the aliases, lib, or from the current directory only. The level up from the current directory path should not be used anywhere.

/* Importing from the lib. */
import Foo from “bar”;

/* Importing from the alias. */
import Foo from “client/components/Foo”;

/* Importing from the current directory */
import Foo from “./Foo”;




/* Importing from the level up from the current directory */
import Foo from “../Foo”;
Braces

Braces are required for all the control structures, even if the body contains only a single statement.

Exception: A simple if statement that can fit entirely on a single line with no wrapping (and that doesn’t have an else) may be kept on a single line with no braces when it improves readability. This is the only case in which a control structure may omit braces and newlines.

/* Bad Examples. */

if (someVeryLongCondition())
  doSomething();


someCondition() && doSomething();


for (let i = 0; i < foo.length; i++) bar(foo[i]);




/* Allowed. */

if (someVeryLongCondition()) {
  doSomething();
}

/* Allowed. */
for (let i = 0; i < foo.length; i++) {
  bar(foo[i]);
}

/* Allowed */
if (shortCondition()) foo();
Descriptive names

The name of the variables/components/functions should be descriptive enough so that a reader can get the intention or purpose of the same from the name only rather than deep diving into the code to find out the exact use.

/* Bad Examples. */

class Abc {
  private def: string[];

  public foo(a) {
    this.def.push(“a”);
  }

  public bar() {
    return this.def.pop()
  }
}

function fooBar(s) {
  try {
   JSON.parse(s);
  } catch (e) {}
}




/* Best Practice. */

class SimpleStack {
  private data: string[];

  public push(a) {
    this.data.push(“a”);
  }

  public pop() {
    return this.data.pop()
  }
}

function jsonParseString(str) {
  try {
   JSON.parse(str);
  } catch (exception) {
    /* Do nothing. */
  }
}




Function vs Arrow Functions

Prefer use function declarations over the function expressions or the arrow function. The arrow functions should be used where it helps readability or when explicit binding to “this” is required.

/* Bad Examples. */

const jsonParse = (inputStr) => {
  try {
     return JSON.parse(inputStr)
  } catch (_exception) {
     return {};
  }
}

const filteredArray = inputArr.filter(function(item) {
   return filterTheItem(item);
}

class MyComponent extends PureComponent {
  ...
  onClick = function() {
    this.setState({clicked: true});
  }




/* Best Practice. */

function jsonParse(inputStr) {
  try {
     return JSON.parse(inputStr)
  } catch (_exception) {
     return {};
  }
}

const filteredArray = inputArr.filter((item) => filterTheItem(item));

class MyComponent extends PureComponent {
  ...
  onClick = () => {
    this.setState({clicked: true});
  }
Functional components vs Class components

Use of functional components should be preferred over the class components unless there are too many event handlers in the component. In such cases class components can be used.

Create a separate file for each component

Never combine two components in a single file however small the component is. Always have different components in different files. Also this applies for the connected component. Mapping state to props and dispatch should be in a separate file then the component itself. Keep the components in the components directory and the connection in the container directory.

//src/App.js

import Navbar from './components/Navbar.js';
import About from './components/About.js';
import Footer from './container/Footer.js';

export default function App() {
 return (
   <>
     <Navbar />
     <About />
     <Footer />    
   </>
 );
}


//src/components/Navbar.js

export default function Navbar() {
 return (
   <nav>
     <h1>Example</h1>
   </nav>
 );
}


//src/components/About.js

export default function About() {
 return (
   <div>
     <h2>About Me.</h2>
   </div>
 );
}


//src/components/Footer.js

export default function Footer() {
 return (
   <footer>
     <h3>This is a footer.</h3>
   </footer>
 );
}


//src/container/Footer.js

import Footer from './components/Footer.js';

const mapDispatchToProps = dispatch => ({
    ...
});

const mapStateToProps = state => ({
    ...
});

export default connect(mapStateToProps, mapDispatchToProps)(Footer);
Reduce the component size

It is a very bad practise to just put all the JSX in a single component instead of creating small components and bifurcating them into separate files according to the functionality that different JSX provides on the UI. Note there should be no exception to this rule. Any component rendering a large number of JSX needs to be broken into several smaller components. 

/* Bad Examples. */

export default function App() {
 return (
   <>
     <nav>
       <h1>Example</h1>
     </nav>
     <div>
       <h2>About Me.</h2>
     </div>
     <footer>
       <h3>This is a footer.</h3>
     </footer>    
   </>
 );
}




/* Best Practice. */

// App.js
import Navbar from './components/Navbar';
import About from './components/About';
import Footer from './components/Footer';

export default function App() {
 return (
   <>
     <NavBar />
     <About />
     <Footer />    
   </>
 );
}

// NavBar.js
export default function NavBar() {
 return (
   <nav>
     <h1>Example</h1>
   </nav>
 );
}

// About.js
export default function About() {
 return (
   <div>
     <h2>About Me.</h2>
   </div>
 );
}

// Footer.js
export default function Footer() {
 return (
   <footer>
     <h3>This is a footer.</h3>
   </footer>
 );
}
Reduce JavaScript in JSX




/* Bad practice. */

import React,{ useState } from 'react';

export default function Form() {
  const [firstName, setFirstName] = useState('');

  return (
    <form>
      <input value={firstName} onChange={event => {
          setFirstName(event.target.value);
          console.log(event.target.value, 'changed!');
        }}/>
    </form>
  );
}




/* Best Practice. */

import React,{ useState } from 'react';

export default function Form() {
 const [firstName, setFirstName] = useState('');
 
 const handleSubmit = useCallback(({target}) => {
  setFirstName(target.value);
  console.log(target.value, 'changed!');
 });

return (
   <form>
     <input value={firstName} onChange={handleSubmit}/>
   </form>
 );
}
Use destructuring




/* Bad practice. */

import React from 'react';

export default function Form(props) {
  return (
   <>
     <h2>Name: {props.person.name}</h2>
     <p>Age: {props.person.age}</p>
     <p>Profession: {props.person.profession} </p>
     <p>Hobby: {props.person.hobby}</p>
   </>
 );
}




/* Best Practice. */

import React from 'react';

export default function Form({name, age, profession, hobby}) {
 return (
   <>
     <h2>Name: {name}</h2>
     <p>Age: {age}</p>
     <p>Profession: {profession} </p>
     <p>Hobby: {hobby}</p>
   </>
 );
}
Conditional rendering




/* Bad practice. */

 //ternary operator with null
 {showModal ? <h1>Modal can only be opened</h1> : null}

 //short-circuiting with not condition
 <>
   {showModal && <h1>Modal was opened</h1>}
   {!showModal && <h1>Modal was closed</h1>}
 </>




/* Best Practice. */

 //short-circuiting
 {showModal && <h1>Modal can only be opened</h1>}

 //ternary operator
 {showModal ? <h1>Modal was opened</h1> : <h1>Modal was closed</h1>}




Wrapping components with memo

All the components must be wrapped with the memo function.

//src/App.js

import React, {memo} from “react”;

import Navbar from './components/Navbar.js';
import About from './components/About.js';
import Footer from './container/Footer.js';

function App() {
 return (
   <>
     <Navbar />
     <About />
     <Footer />    
   </>
 );
}

export default memo(App);
Use semantic tags

The appropriate HTML semantic tags should be used wherever required. Often the texts are wrapped under div or span. Similarly the divs are used as buttons too. Instead h1...h6, p, input or the button tags should be used.

/* Bad practice. */

import React, {memo} from “react”;

function App({someTextProp, onButtonClick}) {
 return (
   <>
     <div>{someTextProp}</div>
     <div onClick={onButtonClick}>Click Me!</div>   
   </>
 );
}




/* Best Practice. */

import React, {memo} from “react”;

function App({someTextProp, onButtonClick}) {
 return (
   <>
     <p>{someTextProp}</p>
     <button onClick={onButtonClick}>Click Me!</button>   
   </>
 );
}




Compound Components Pattern

Sometimes the component has to be created such that it has to have too many props leading to either the props drilling or a big component having too much of JSX. In these scenarios one should consider the compound components pattern.

// Either the components with these many or more props will result in 
// props drilling or single big component problems. This also makes the
// code less readable.

import React, {memo, useCallback} from "react";

import { Counter } from "./Counter";

function Usage() {
  const handleChangeCounter = useCallback((count) => {
    console.log("count", count);
  }, []);

  return (
    <Counter
      label=”Label”
      max={10}
      min={1}
      iconDecrement=”minus”
      iconIncrement=”plus”
      onChange={handleChange}
      value={2}
      cycleAround
      displayAs="RomanNumerals"
    />
  );
}

export default memo(Usage);




// Instead use the compound component pattern. It will result in
// several smaller components and without props drilling while also
// preserving the code readability.

import React, {memo, useCallback} from "react";

import { Counter } from "./Counter";

function Usage() {
  const handleChangeCounter = useCallback((count) => {
    console.log("count", count);
  }, []);

  return (
    <Counter value={2} onChange={handleChangeCounter}>
      <Counter.Icon icon="minus" />
      <Counter.Label>Counter</Counter.Label>
      <Counter.Count max={10} min={1} cycleAround />
      <Counter.Icon icon="plus" />
      <Counter.Display as="RomanNumerals" />
    </Counter>
  );
}

export default memo(Usage);




Remove inline functions in props
// Bad practise.

import React, {memo, useState} from "react";

import { Counter } from "./Counter";

function Usage() {
  const [count, setCount] = useState(2);

  return (
    <Counter
      onChange={(newCount) => setCount(newCount)}
      value={count}
    />
  );
}

export default memo(Usage);




// Good Practise.

import React, {memo, useState, useCallback} from "react";

import { Counter } from "./Counter";

function Usage() {
  const [count, setCount] = useState(2);

  const onChange = useCallback((newCount) => {
    setCount(newCount);
  }, [setCount]);

  return (
    <Counter
      onChange={onChange}
      value={count}
    />
  );
}

export default memo(Usage);




Components Directory structure.


Creating a single file for a component is not a good practise even if it is a very small component. Each component should be a directory with the component.ts and index.ts files

Try segregating large components into smaller child components in separate file, separate file for connecting the component with redux, separate file for creating styled components, separate files for the logic, helper and the util functions and the separate file for constants or hooks. This will make each TS, SASS file more readable and manageable.

For example if we are creating a component named Export.

Instead of creating just one file as Export.ts create a directory named Export and in that directory we can have following files index.ts, Export.ts, helpers or helper.ts, components, constants or constants.ts.

index.ts will just have mostly one line

export {default} from "./Export";


Export.ts will be main component.

helpers will be a directory which have many separate helper files that contains the logic. This is only when there are many helper functions. Usually when there are many helper functions they themselves can be categories into multiple meaningful helper files like apiHelper.ts which will contain the helper functions for making api calls, transformData.ts which will contain the helper function for transforming the data from one format to another are some of the examples. And all these helper files can be managed under one helpers directory.

else if there are not many helper function and all such functions are intended for one kind of use cases only then single file helper.ts can be used to house all the required helper functions. Same goes for the directory constants or constants.ts And all the test cases for the helper files can be added in __test__ directory.

components directory will house the child components that will be used inside only Export.ts component so that the Export.ts file can be trimmed down into manageable and readable TS file.

Note all the code in helpers, components, constants etc. should only be used by Export.ts. if it is used outside the Export, then the same helpers, components, constants etc. should be migrated to global constants, global components etc.

Directory structure for reference


├── src                                            # Application source code
│ ├── components/route                             # Global/Local components or route directory
│ │ ├── Export                                     # Export component
│ │ │ ├── components                               # Specific child components only used in Export.ts
│ │ │ ├── index.ts                                 # Directory entry and export controller
│ │ │ ├── Export.ts                                # Main Export component
│ │ │ ├── helper.ts/helpers(directory)             # Specific helpers only used in Export.ts
│ │ │ ├── constants.ts/constants(directory)        # Specific constants only used in Export.ts
│ │ │ └── etc...                                   # Etc. specifically required in Export.js
│ │ ├── OtherComponent                             # Some Other Component Directory
