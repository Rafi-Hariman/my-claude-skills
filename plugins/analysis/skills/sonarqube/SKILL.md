---
name: sonarqube
description: Check code against SonarQube quality standards for TypeScript, HTML, and CSS. Focus on projects/uiigateway/projectname directory.
tools: Read, Glob, Bash
---

You are a SonarQube code quality assistant. When invoked, you will check code against SonarQube standards for:
- TypeScript
- HTML
- CSS
---

## CSS Standards

### Blocker Severity
| Rule | Type | Description |
|------|------|-------------|
| "calc" operands should be correctly spaced | Bug | Proper spacing required in calc() expressions |
| Color definitions should be valid | Bug | Invalid color values detected |
| CSS properties should be valid | Bug | Unknown or invalid CSS properties used |
| "linear-gradient" directions should be valid | Bug | Invalid gradient direction values |
| Single line comment syntax should not be used | Bug | CSS doesn't support single-line comments with // |
| Units should be valid | Bug | Invalid CSS units detected |

### High Severity
| Rule | Type | Description |
|------|------|-------------|
| "linear-gradient" directions should be valid | Bug | Invalid gradient direction values (4) |
| Expressions within "calc" should be valid | Bug | Invalid calc() expressions |
| Selectors should be known | Bug | Unknown CSS selectors used |
| Shorthand properties that override related longhand properties should be avoided | Bug | Conflicting property declarations |
| Selectors of lower specificity should come before overriding selectors of higher specificity | Code Smell | Specificity order issue |

### Medium Severity
| Rule | Type | Description |
|------|------|-------------|
| "!important" should not be used on "keyframes" | Bug | Avoid !important in keyframes |
| "at-rules" should be valid | Bug | Invalid CSS at-rules |
| CSS files should not be empty | Code Smell | Empty CSS file detected |
| Duplicate imports should be removed | Code Smell | Duplicate @import statements |
| Duplicated font names should be removed | Code Smell | Duplicate font-family names |
| Empty blocks should be removed | Code Smell | Empty CSS blocks |
| Font declarations should contain at least one generic font family | Bug | Missing generic font fallback |
| Media features should be valid | Bug | Invalid media features |
| Multi-line comments should not be empty | Code Smell | Empty CSS comments |
| Properties should not be duplicated | Bug | Duplicate property declarations |
| Pseudo-class selectors should be valid | Bug | Invalid pseudo-class selectors |
| Pseudo-element selectors should be valid | Bug | Invalid pseudo-element selectors |
| Sections of code should not be commented out | Code Smell | Commented-out code detected |
| Selectors should not be duplicated | Code Smell | Duplicate selectors |
| Strings should not contain new lines | Code Smell | Multi-line strings without proper escaping |

### Low Severity
| Rule | Type | Description |
|------|------|-------------|
| Extra semicolons should be removed | Code Smell | Unnecessary semicolons detected |

---

## HTML Standards

### Blocker Severity
| Rule | Type | Tags |
|------|------|------|
| Disallowed "taglibs" should not be used | Code Smell | - |
| Some Java packages or classes should not be used in JSP files | Code Smell | jsp-jsf |

### High Severity
| Rule | Type | Tags |
|------|------|------|
| Table cells should reference their headers | Bug | accessibility, wcag2-a |
| Tables used for layout should not include semantic markup | Bug | accessibility, wcag2-a |

### Medium Severity
| Rule | Type | Tags |
|------|------|------|
| "<!DOCTYPE>" declarations should appear before "<html>" tags | Bug | user-experience |
| "<input>", "<select>" and "<textarea>" tags should be labeled | Bug | accessibility, wcag2-a |
| "<li>" and "<dt>" item tags should be in "<ul>", "<ol>" or "<dl>" container tags | Bug | - |
| "<script>...</script>" elements should not be nested | Bug | - |
| "<th>" tags should have "id" or "scope" attributes | Bug | accessibility, wcag2-a |
| "<title>" should be present in all pages | Bug | user-experience |
| All HTML tags should be closed | Bug | - |
| Anchor tags should not be used as buttons | Code Smell | accessibility |
| Animate entities or properties should not be persisted | Code Smell | android |
| ARIA properties in DOM elements should have valid values | Code Smell | accessibility |
| Attributes deprecated in HTML5 should not be used | Code Smell | html5, obsolete |
| Attributes should be quoted using double quotes rather than single ones | Code Smell | convention |
| DOM elements should not use the "accesskey" property | Code Smell | accessibility |
| DOM elements should use the "autocomplete" attribute correctly | Code Smell | accessibility |
| DOM elements with ARIA role should only have supported properties | Code Smell | accessibility |
| DOM elements with ARIA roles should have a valid non-abstract role | Code Smell | accessibility |
| DOM elements with ARIA roles should have the required properties | Code Smell | accessibility |
| DOM elements with the `aria-activedescendant` property should be accessible via the tab key | Code Smell | accessibility |
| Dynamic includes should not be used | Code Smell | performance, jsp-jsf |
| Elements deprecated in HTML5 should not be used | Bug | html5, obsolete |
| Elements with an interactive role should support focus | Code Smell | accessibility |
| Favicons should be used in all pages | Code Smell | user-experience |
| Files should not be too complex | Code Smell | - |
| Files should not have too many lines | Code Smell | brain-overload |
| Focusable elements should not have "aria-hidden" attribute | Code Smell | accessibility |
| Heading elements should have accessible content | Code Smell | accessibility |
| HTML "<table>" should not be used for layout purposes | Code Smell | accessibility |
| HTML comments should be removed | Code Smell | - |
| Image tags should have "width" and "height" attributes | Bug | user-experience |
| Image, area and button with image elements should have an "alt" attribute | Code Smell | accessibility, wcag2-a |
| Images should have a non-redundant alternate description | Code Smell | accessibility |
| Interactive DOM elements should not have non-interactive ARIA roles | Code Smell | accessibility |
| JavaScript scriptlets should not have too many lines of code | Code Smell | brain-overload |
| JSF expressions should be syntactically valid | Bug | - |
| JSP expressions should not be used | Code Smell | jsp-jsf, obsolete |
| Label elements should have a text label and an associated control | Code Smell | accessibility |
| Labels should be defined in the resource bundle | Bug | user-experience |
| Lines should not be too long | Code Smell | convention |
| Links should not directly target images | Code Smell | - |
| Links should not target "#" or "javascript:void(0)" | Code Smell | user-experience |
| Links with identical texts should have identical targets | Code Smell | accessibility |
| Meta tags should not be used to refresh or redirect | Code Smell | user-experience |
| Multiple "page" directives should not be used | Code Smell | convention, jsp-jsf |
| No ARIA role or property for unsupported DOM elements | Code Smell | accessibility |
| No redundant ARIA role | Code Smell | accessibility |
| Non-interactive DOM elements should not have an interactive handler | Code Smell | accessibility |
| Non-interactive DOM elements should not have interactive ARIA roles | Code Smell | accessibility |
| Non-interactive DOM elements should not have the `tabIndex` property | Code Smell | accessibility |
| Non-interactive elements shouldn't have event handlers | Code Smell | accessibility |
| Prefer tag over ARIA role | Code Smell | accessibility |
| Sections of code should not be commented out | Code Smell | - |
| Server-side image maps ("ismap" attribute) should not be used | Bug | accessibility |
| Tabulation characters should not be used | Code Smell | convention |
| The "style" attribute should not be used | Code Smell | - |
| Track lack of copyright and license headers | Code Smell | convention |
| Track lack of required an element with the required "id" | Code Smell | convention |
| Track lack of required attributes | Code Smell | - |
| Track lack of required child elements | Code Smell | - |
| Track lack of required parent elements | Code Smell | - |
| Track presence of forbidden parent element | Code Smell | - |
| Track uses of "FIXME" tags | Code Smell | - |
| Track uses of "TODO" tags | Code Smell | cwe |
| Track uses of disallowed attributes | Code Smell | - |
| Track uses of disallowed child elements | Code Smell | - |
| Track uses of disallowed elements | Code Smell | - |
| Videos should have subtitles | Code Smell | accessibility, html5 |
| Web pages should not contain absolute URIs | Code Smell | - |
| White space should be used in JSP/JSF tags | Code Smell | convention, jsp-jsf |

### Low Severity
| Rule | Type | Tags |
|------|------|------|
| "<fieldset>" tags should contain a "<legend>" | Bug | Deprecated |
| "<frames>" should have a "title" attribute | Bug | accessibility |
| "<html>" element should have a language attribute | Bug | accessibility, wcag2-a |
| "<object>" tags should provide an alternative content | Code Smell | accessibility |
| "<strong>" and "<em>" tags should be used | Bug | Deprecated |
| "<table>" tags should have a description | Bug | Deprecated, accessibility |
| "aria-label" or "aria-labelledby" attributes should be used to differentiate similar elements | Code Smell | accessibility, wcag2-a |
| "tabIndex" values should be 0 or -1 | Code Smell | accessibility |
| Anchors should contain accessible content | Code Smell | accessibility |
| Flash animations should be embedded using both "<object>" and "<embed>" | Bug | cross-browser |
| Flash animations should be embedded using the window mode | Bug | accessibility |
| Heading tags should be used consecutively from "H1" to "H6" | Code Smell | Deprecated, accessibility |

### Security Hotspots
| Rule | Severity | Tags |
|------|----------|------|
| Authorizing an opened window to access back to the originating window is security-sensitive | Low | cwe, phishing |
| Disabling auto-escaping in template engines is security-sensitive | Medium | cwe |
| Using remote artifacts without integrity checks is security-sensitive | Low | cwe |

---

## TypeScript Standards

### Blocker Severity - Vulnerabilities
| Rule | Severity | Tags |
|------|----------|------|
| A new session should be created during user authentication | High | cwe |
| Cipher algorithms should be robust | High | cwe, privacy |
| Cryptographic keys should be robust | High | cwe, privacy |
| Creating public APIs is security-sensitive | Blocker | cwe, aws |
| Disabling auto-escaping in template engines is security-sensitive | Medium | cwe |
| Disabling Vue.js built-in escaping is security-sensitive | Blocker | Deprecated, cwe |
| Encryption algorithms should be used with secure mode and padding scheme | High | cwe, privacy |
| File uploads should be restricted | High | cwe, express.js |
| Hard-coded passwords are security-sensitive | Blocker | cwe |
| Hard-coded secrets are security-sensitive | Blocker | cwe |
| JWT should be signed and verified with strong cipher algorithms | High | cwe, privacy |
| Origins should be verified during cross-origin communications | High | cwe, html5 |
| Policies authorizing public access to resources are security-sensitive | Blocker | cwe, aws |
| Policies granting access to all resources of an account are security-sensitive | Blocker | cwe, aws |
| Policies granting all privileges are security-sensitive | Blocker | cwe, aws |
| Server certificates should be verified during SSL/TLS connections | High | cwe, privacy |
| Server hostnames should be verified during SSL/TLS connections | High | cwe, privacy |
| "switch" statements should not contain non-case labels | Blocker | suspicious |
| Variables declared with "var" should be declared before they are used | Blocker | pitfall |
| Weak SSL/TLS protocols should not be used | High | cwe, privacy |
| Web SQL databases should not be used | Blocker | Deprecated, type-dependent |
| Wildcard imports should not be used | High | es2015, pitfall |
| XML parsers should not be vulnerable to XXE attacks | Blocker | cwe |

### Blocker Severity - Code Quality
| Rule | Type | Tags |
|------|------|------|
| Assertions should be complete | Code Smell | tests |
| "switch" statements should not have too many "case" clauses | Code Smell | brain-overload |
| Test files should contain at least one test case | Code Smell | node, tests |
| Tests should include assertions | Code Smell | chai, tests |
| Track lack of copyright and license headers | Code Smell | convention |

### High Severity - Bugs
| Rule | Tags |
|------|------|
| "Array.prototype.sort()" and "Array.prototype.toSorted()" should use a compare function | type-dependent, bad-practice |
| "Array.reduce()" calls should include an initial value | type-dependent |
| "case" and "default" clauses should not contain lexical declarations | - |
| "delete" should be used only with object properties | type-dependent |
| "for...in" loops should filter properties before acting on them | - |
| "if/else if" chains and "switch" cases should not have the same condition | unused, pitfall |
| "NaN" should not be used in comparisons | - |
| "new" should only be used with functions and classes | type-dependent |
| "setState" should use a callback when referencing the previous state | react |
| "super()" should be invoked appropriately | - |
| All branches in a conditional structure should not have exactly the same implementation | - |
| All code should be reachable | cwe, unused |
| Alternation in regular expressions should not contain empty alternatives | regex, type-dependent |
| Alternatives in regular expressions should be grouped when used with anchors | regex, type-dependent |
| A "for" loop update clause should move the counter in the right direction | - |
| Binary expressions should not always return the same value | - |
| Bitwise operators should not be used in boolean contexts | - |
| Built-in objects should not be overridden | confusing |
| Collection elements should not be replaced unconditionally | suspicious |
| Collection size and array length comparisons should make sense | type-dependent |
| Comma and logical OR operators should not be used in switch cases | - |
| Comments inside JSX expressions should be enclosed in curly braces | react |
| Comma operator should not be used | - |
| Constructors should be declared inside interfaces | - |
| Destructuring patterns should not be empty | - |
| Empty character classes should not be used | type-dependent |
| Empty collections should not be accessed or iterated | - |
| Errors should not be created without being thrown | error-handling |
| Exclusive tests should not be committed to version control | - |
| Function declarations should not be made within blocks | cross-browser, user-experience |
| Getters and setters should access the expected fields | pitfall |
| Generators should explicitly "yield" a value | api-design, es2015 |
| Identical expressions should not be used on both sides of a binary operator | suspicious |
| "if" statements should be preferred over "switch" when simpler | bad-practice |
| Initial values of parameters, caught exceptions, and loop variables should not be ignored | - |
| In React "this.state" should not be mutated directly | react |
| Literals should not be used as functions | - |
| Loops with at most one iteration should be refactored | bad-practice, confusing |
| Member names should not be duplicated within a class or object literal | pitfall |
| Mouse events should have corresponding keyboard events | accessibility, react |
| Non-empty statements should change control flow or have at least one side-effect | cwe, unused |
| Non-existent operators '=+', '=-' and '=!' should not be used | - |
| "Number.isNaN()" should be used to check for "NaN" value | type-dependent |
| Numbers should not lose precision | precision, numbers |
| Objects should not be created to be dropped immediately without being used | - |
| Optional chaining should not be used if returning "undefined" throws an error | es2015, bad-practice |
| Parameters should be passed in the correct order | type-dependent |
| Parentheses should be used when negating "in" and "instanceof" operations | - |
| Promise rejections should not be caught by "try" blocks | type-dependent |
| Promises should not be misused | async, type-dependent |
| React "render" functions should return a value | react |
| React components should not render non-boolean condition values | type-dependent, react |
| React Hooks should be properly called | react |
| React state setter function should not be called with its matching state variable | react |
| React's useState hook should not be used directly in the render function or body of a component | react |
| Regex quantifiers and character classes should be used concisely | regex, type-dependent |
| Regular expression quantifiers and character classes should be used concisely | regex, type-dependent |
| Regular expressions should be syntactically valid | type-dependent |
| Regular expressions should not contain control characters | regex, type-dependent |
| Repeated patterns in regular expressions should not match the empty string | type-dependent |
| Replacement strings should reference existing regular expression groups | regex, type-dependent |
| Results of operations on strings should not be ignored | Deprecated, type-dependent |
| Return values from functions without side effects should not be ignored | type-dependent |
| "String.prototype.trim()" should not be used on non-strings | type-dependent |
| "switch" statements should not be nested | pitfall |
| Table cells should reference their headers | accessibility, wcag2-a |
| Tables should have headers | accessibility, wcag2-a |
| Template literal placeholder syntax should not be used in regular strings | - |
| Tests should not be skipped without providing a reason | tests, bad-practice |
| Tests should not execute any code after "done()" is called | tests, unpredictable |
| The base should be provided to "parseInt" | user-experience |
| The return value of void functions should not be used | - |
| "this" should not be used in functional components | react |
| Track uses of "FIXME" tags | cwe |
| Type intersections should use meaningful types | type-dependent, pitfall |
| Unicode Grapheme Clusters should be avoided inside regex character classes | regex, type-dependent |
| Variables should not be self-assigned | - |
| Values should not be uselessly incremented | unused |

### High Severity - Code Smells
| Rule | Tags |
|------|------|
| All enum members should be literals | enum |
| An open curly brace should be located at the end of a line | Deprecated |
| Array indexes should be numeric | type-dependent, bad-practice |
| Assertions should not be given twice the same argument | tests |
| Assignments should not be made from within sub-expressions | cwe, suspicious |
| Boolean checks should not be inverted | pitfall |
| Boolean expressions should not be gratuitous | cwe, suspicious |
| Boolean literals should not be used in comparisons | clumsy |
| Braces and parentheses should be used consistently with arrow functions | convention, es2015 |
| Calls to ".call()" and ".apply()" methods should not be redundant | - |
| Class names should comply with a naming convention | convention |
| Class methods should be used instead of "prototype" assignments | es2015, type-dependent |
| Cognitive Complexity of functions should not be too high | brain-overload, architecture |
| Comparison operators should not be used with strings | type-dependent |
| Conditionals should start on new lines | suspicious |
| Constructor names should start with an upper case letter | convention |
| Constructors should not contain asynchronous operations | - |
| Constructors should not return values | - |
| Control flow statements "if", "for", "while", "switch" and "try" should not be nested too deeply | brain-overload |
| Control structures should use curly braces | pitfall |
| Cyclomatic Complexity of functions should not be too high | brain-overload, architecture |
| Default export names and file names should match | convention, es2015 |
| Disabled git lint rules should be re-enabled | git |
| Destructuring syntax should be used for assignments | es2015, clumsy |
| Expressions should not be too complex | brain-overload |
| Extra boolean casts should be removed | redundant |
| Fields that are only assigned in the constructor should be "readonly" | type-dependent, confusing |
| Files should not have too many lines of code | brain-overload, architecture |
| Formatting SQL queries is security-sensitive | cwe, bad-practice |
| Function and method names should comply with a naming convention | convention |
| Function call arguments should not start on new lines | suspicious |
| Function parameters with default values should be last | es2015 |
| Function returns should not be invariant | - |
| Function types should be preferred | function, type |
| Functions should not be defined inside loops | suspicious |
| Functions should not be empty | suspicious |
| Functions should not be nested too deeply | brain-overload |
| Functions should not have identical implementations | suspicious, confusing |
| Functions should not have too many lines of code | brain-overload, architecture |
| Functions should not have too many parameters | brain-overload |
| Functions should use "return" consistently | api-design, confusing |
| Imports should not use absolute paths | paths, pitfall |
| Increment (++) and decrement (--) operators should not be used in a method call or mixed with other operators in an expression | - |
| Jump statements should not be redundant | redundant, clumsy |
| Lines should not be too long | Deprecated |
| Local variables should not be declared and then immediately returned or thrown | clumsy |
| Loop counters should not be assigned within the loop body | - |
| Loops should not contain more than a single "break" or "continue" statement | brain-overload |
| Magic numbers should not be used | brain-overload |
| Member names should not be duplicated within a class or object literal | pitfall |
| Mergeable "if" statements should be combined | clumsy |
| Method overloads should be grouped together | convention |
| Methods should not contain selector parameters | design, type-dependent |
| Module should not import itself | architecture |
| Multiline blocks should be enclosed in curly braces | cwe |
| Multiline string literals should not be used | bad-practice |
| Names of regular expressions named groups should be used | regex, type-dependent |
| Nested blocks of code should not be left empty | suspicious |
| Nested code blocks should not be used | bad-practice |
| Non-interactive DOM elements should not have the `tabIndex` property | accessibility, react |
| Object spread syntax should be used instead of "Object.assign" | es2018 |
| Objects and classes converted or coerced to strings should define a "toString()" method | string, type-dependent |
| Octal values should not be used | pitfall |
| Only "while", "do", "for" and "switch" statements should be labelled | pitfall |
| Quote character should be used consistently | convention |
| Redundant casts and non-null assertions should be avoided | type-dependent, redundant |
| Redundant pairs of parentheses should be removed | Deprecated |
| Regular expressions should not be too complicated | regex, type-dependent |
| Regular expressions should not contain empty groups | regex, type-dependent |
| Regular expressions should not contain multiple spaces | regex, type-dependent |
| Regular expressions with the global flag should be used with caution | regex |
| Reluctant quantifiers in regular expressions should be followed by an expression that can't match the empty string | regex, type-dependent |
| Shorthand object properties should be grouped at the beginning or end of an object declaration | convention, es2015 |
| Single-character alternations in regular expressions should be replaced with character classes | regex, type-dependent |
| Sparse arrays should not be created with extra commas | suspicious |
| Special identifiers should not be bound or assigned | pitfall |
| Spread syntax should be used instead of "apply()" | - |
| Statements should be on separate lines | Deprecated |
| Statements should end with semicolons | Deprecated |
| String literals should not be duplicated | design |
| String references should not be used | react |
| Strings and non-strings should not be added | type-dependent, confusing |
| Switch cases should end with an unconditional "break" statement | cwe, suspicious |
| Tabulation characters should not be used | Deprecated |
| Template literals should not be nested | brain-overload, confusing |
| Template strings should be used instead of concatenation | es2015, clumsy |
| Tests should be stable | tests, unpredictable |
| Tests should check which exception is thrown | tests, chai |
| The global "this" object should not be used | confusing |
| The ternary operator should not be used | brain-overload |
| Ternary operators should not be nested | confusing |
| Track comments matching a regular expression | Rule Template |
| Track uses of "NOSONAR" comments | bad-practice |
| Track uses of "TODO" tags | cwe |
| Trailing commas should not be used | Deprecated |
| Two branches in a conditional structure should not have exactly the same implementation | design, suspicious |
| Type assertions should use "as" | confusing |
| Type constituents of unions and intersections should not be redundant | type-dependent, redundant |
| Union and intersection types should not include duplicated constituents | - |
| Union types should not have too many elements | brain-overload |
| Unchanged variables should be marked as "const" | es2015 |
| Unnecessary calls to ".bind()" should not be used | - |
| Unnecessary character escapes should be removed | - |
| Unnecessary constructors should be removed | - |
| Unnecessary type constraints should be removed | - |
| Unused assignments should be removed | cwe, unused |
| Unused function parameters should be removed | unused |
| Unused private class members should be removed | unused |
| Use Object.hasOwn static method instead of hasOwnProperty | es2022 |
| User-defined JSX components should use Pascal case | react |
| Variable, property and parameter names should comply with a naming convention | convention |
| Variables should be used in the blocks where they are declared | pitfall |
| Variables should not be shadowed | suspicious, pitfall |
| Wrapper objects should not be used for primitive types | pitfall |

### Medium Severity - Code Smells
| Rule | Tags |
|------|------|
| "===" and "!==" should be used instead of "==" and "!=" | suspicious |
| "arguments" should not be accessed directly | api-design, es2015 |
| "arguments.caller" and "arguments.callee" should not be used | obsolete |
| "Array.reduce()" calls should include an initial value | type-dependent |
| "as const" assertions should be preferred | assertion |
| "await" should not be used redundantly | redundant |
| "catch" clauses should do more than rethrow | error-handling, unused |
| "children" and "dangerouslySetInnerHTML" should not be used together | react |
| "continue" should not be used | bad-practice |
| "default" clauses should be last | - |
| "delete" should not be used on arrays | type-dependent |
| "for in" should not be used with iterables | type-dependent |
| "for of" should be used with Iterables | clumsy |
| "for" loop increment clauses should modify the loops' counters | confusing |
| "for...in" loops should filter properties before acting on them | - |
| "if ... else if" constructs should end with "else" clauses | - |
| "if/else if" chains and "switch" cases should not have the same condition | unused, pitfall |
| "import" should be used to include external code | convention, es2015 |
| "in" should not be used on arrays | type-dependent, pitfall |
| "indexOf" checks should not be for positive numbers | type-dependent, suspicious |
| "module" should not be used | obsolete |
| "module" should not be used when ECMAScript modules are available | obsolete |
| "Number.isNaN()" should be used to check for "NaN" value | type-dependent |
| "RegExp.exec()" should be preferred over "String.match()" | regex, type-dependent |
| "shouldComponentUpdate" should not be defined when extending "React.PureComponent" | react |
| "strict" mode should be used with caution | cross-browser, user-experience |
| "switch" statements should have "default" clauses | cwe, type-dependent |
| "tabIndex" values should be 0 or -1 | accessibility, react |
| "this" should not be assigned to variables | clumsy |
| "undefined" should not be assigned | suspicious |
| "undefined" should not be passed as the value of optional parameters | type-dependent, confusing |
| "void" should not be used | type-dependent, confusing |
| **proto** property should not be used | type-dependent, confusing |
| A "while" loop should be used instead of a "for" loop | clumsy |
| A conditionally executed single line should be denoted by indentation | - |
| All "defaultProps" should have non-required PropTypes | react |
| All branches in a conditional structure should not have exactly the same implementation | - |
| All code should be reachable | cwe, unused |
| All enum members should be literals | enum |
| Allow confidential information to be logged is security-sensitive | privacy |
| Anchors should contain accessible content | accessibility, react |
| Anchor tags should not be used as buttons | accessibility, react |
| ARIA properties in DOM elements should have valid values | accessibility, react |
| Array constructors should not be used | suspicious |
| Array-mutating methods should not be used misleadingly | type-dependent |
| Assertion arguments should be passed in the correct order | chai, tests |
| Assignments should not be made from within sub-expressions | cwe, suspicious |
| Assignments should not be redundant | redundant |
| Boolean expressions should not be gratuitous | cwe, suspicious |
| Character classes in regular expressions should not contain only one character | regex, type-dependent |
| Character classes in regular expressions should not contain the same character twice | regex, type-dependent |
| Class names should comply with a naming convention | convention |
| Classes should not be empty | clumsy |
| Collection contents should be used | unused, suspicious |
| Comments should not be located at the end of lines of code | Deprecated |
| Comma and logical OR operators should not be used in switch cases | - |
| Comma operator should not be used | - |
| Default type parameters should be omitted | type-dependent, clumsy |
| Destructuring syntax should be used for assignments | es2015, clumsy |
| DOM elements should not use the "accesskey" property | accessibility, react |
| DOM elements should use the "autocomplete" attribute correctly | accessibility, react |
| DOM elements with ARIA role should only have supported properties | accessibility, react |
| DOM elements with ARIA roles should have a valid non-abstract role | accessibility, react |
| DOM elements with ARIA roles should have the required properties | accessibility, react |
| DOM elements with the `aria-activedescendant` property should be accessible via the tab key | accessibility, react |
| Elements with an interactive role should support focus | accessibility, react |
| Empty character classes should not be used | type-dependent |
| Empty collections should not be accessed or iterated | - |
| Ends of strings should be checked with "startsWith()" and "endsWith()" | type-dependent |
| Enum member values should be either all initialized or none | - |
| Enum members should not mix value types | type-dependent, enum |
| Enum values should be unique | - |
| Equality operators should not be used in "for" loop termination conditions | cwe, suspicious |
| Exceptions should not be ignored | cwe, error-handling |
| Extra semicolons should be removed | Deprecated |
| Fields that are only assigned in the constructor should be "readonly" | type-dependent, confusing |
| Files should not have too many lines of code | brain-overload, architecture |
| Focusable elements should not have "aria-hidden" attribute | accessibility, react |
| Function and method names should comply with a naming convention | convention |
| Function call arguments should not start on new lines | suspicious |
| Function parameters with default values should be last | es2015 |
| Function returns should not be invariant | - |
| Functions should not be empty | suspicious |
| Functions should not have identical implementations | suspicious, confusing |
| Functions should not have too many lines of code | brain-overload, architecture |
| Functions should not have too many parameters | brain-overload |
| Functions should use "return" consistently | api-design, confusing |
| Heading elements should have accessible content | accessibility, react |
| HTML "<table>" should not be used for layout purposes | accessibility, react |
| HTML elements should have a valid language attribute | accessibility, react |
| If statements should not be the only statement in else blocks | - |
| iFrames must have a title | accessibility, react |
| Image, area, button with image and object elements should have an alternative text | accessibility, react |
| Images should have a non-redundant alternate description | accessibility, react |
| Import variables should not be reassigned | bad-practice, confusing |
| Imports from the same module should be merged | es2015 |
| Imports should not use absolute paths | paths, pitfall |
| In React "this.state" should not be mutated directly | react |
| Interactive DOM elements should not have non-interactive ARIA roles | accessibility, react |
| Interfaces should not be empty | - |
| JavaScript parser failure | suspicious |
| JSX elements should not use unknown properties and attributes | react |
| JSX list components keys should match up between renders | performance, react |
| JSX list components should have a key property | performance, react |
| JSX list components should not use array indexes as key | performance, react |
| JSX special characters should be escaped | react |
| Label elements should have a text label and an associated control | accessibility, react |
| Labels should not be used | confusing |
| Lines should not be too long | Deprecated |
| Lines should not end with trailing whitespaces | Deprecated |
| Literals should not be thrown | error-handling, api-design |
| Literals should not be used for promise rejection | - |
| Local variables should not be declared and then immediately returned or thrown | clumsy |
| Loop counters should not be assigned within the loop body | - |
| Loops should not contain more than a single "break" or "continue" statement | brain-overload |
| Media elements should have captions | accessibility, react |
| Mergeable "if" statements should be combined | clumsy |
| Method overloads should be grouped together | convention |
| Methods should not contain selector parameters | design, type-dependent |
| Module should not import itself | architecture |
| Mouse events should have corresponding keyboard events | accessibility, react |
| Multiline blocks should be enclosed in curly braces | cwe |
| No ARIA role or property for unsupported DOM elements | accessibility, react |
| No redundant ARIA role | accessibility, react |
| Non-interactive DOM elements should not have an interactive handler | accessibility, react |
| Non-interactive DOM elements should not have interactive ARIA roles | accessibility, react |
| Non-interactive DOM elements should not have the `tabIndex` property | accessibility, react |
| Non-interactive elements shouldn't have event handlers | accessibility, react |
| Non-null assertions should not be used | unpredictable |
| Non-null assertions should not be used misleadingly | confusing, clumsy |
| Nullish coalescing should be preferred | type-dependent, nullish-coalescing |
| Object literal shorthand syntax should be used | convention, es2015 |
| Object spread syntax should be used instead of "Object.assign" | es2018 |
| Objects and classes converted or coerced to strings should define a "toString()" method | string, type-dependent |
| Octal escape sequences should not be used | - |
| Octal values should not be used | pitfall |
| Only "while", "do", "for" and "switch" statements should be labelled | pitfall |
| Optional boolean parameters should have default value | - |
| Optional chaining should be preferred | - |
| Optional property declarations should not use both '?' and 'undefined' syntax | type-dependent, redundant |
| Parameters should be passed in the correct order | type-dependent |
| Prefer tag over ARIA role | accessibility, react |
| Prefer the return type "this" in fluent interfaces | api-design, type-dependent |
| Primitive return types should be used | type-dependent, proficiency |
| Primitive types should be omitted from initialized or defaulted declarations | finding, clumsy |
| Property getters and setters should come in pairs | pitfall |
| Prototypes of builtin objects should not be modified | - |
| Public "static" fields should be read-only | cwe |
| Quotes for string literals should be used consistently | Deprecated |
| React "children" should not be passed as prop | react |
| React components should not be nested | performance, react |
| React Context Provider values should have stable identities | performance, react |
| React legacy lifecycle methods should not be used | react |
| React props should be read-only | type-dependent, react |
| React's "findDOMNode" should not be used | react |
| React's "isMounted" should not be used | react |
| Redundant type aliases should not be used | - |
| Regular expression literals should be used when possible | regex |
| Renaming import, export, and destructuring assignments should not be to the same name | - |
| Sections of code should not be commented out | unused |
| Shorthand object properties should be grouped at the beginning or end of an object declaration | convention, es2015 |
| Shorthand promises should be used | proficiency |
| Single-character alternations in regular expressions should be replaced with character classes | regex, type-dependent |
| Spacing between inline elements should be explicit | react |
| Special identifiers should not be bound or assigned | pitfall |
| Spread syntax should be used instead of "apply()" | - |
| Standard outputs should not be used directly to log anything | bad-practice, user-experience |
| Statements should be on separate lines | Deprecated |
| String literals should not be duplicated | design |
| String references should not be used | react |
| Strings and non-strings should not be added | type-dependent, confusing |
| Switch cases should end with an unconditional "break" statement | cwe, suspicious |
| Tabulation characters should not be used | Deprecated |
| Template literals should not be nested | brain-overload, confusing |
| Test files should contain at least one test case | node, tests |
| Tests should be stable | tests, unpredictable |
| Tests should check which exception is thrown | tests, chai |
| Tests should not execute any code after "done()" is called | tests, unpredictable |
| Tests should not be skipped without providing a reason | tests, bad-practice |
| The "any" type should not be used | pitfall |
| The return value of "ReactDOM.render" should not be used | react |
| The return value of "useState" should be destructured and named symmetrically | react |
| The ternary operator should not be used | brain-overload |
| Track comments matching a regular expression | Rule Template |
| Track uses of "FIXME" tags | cwe |
| Track uses of "TODO" tags | cwe |
| Trailing commas should be used | Deprecated |
| Two branches in a conditional structure should not have exactly the same implementation | design, suspicious |
| Type aliases should be used | proficiency |
| Unchanged variables should be marked as "const" | es2015 |
| Unnecessary calls to ".bind()" should not be used | - |
| Unnecessary character escapes should be removed | - |
| Unnecessary constructors should be removed | - |
| Unnecessary imports should be removed | es2015, type-dependent |
| Unused assignments should be removed | cwe, unused |
| Unused function parameters should be removed | unused |
| Unused private class members should be removed | unused |
| Unused React typed props should be removed | react |
| User-defined JSX components should use Pascal case | react |
| Users should not use internal APIs | gradle, architecture |
| Variable, property and parameter names should comply with a naming convention | convention |
| Variables should be used in the blocks where they are declared | pitfall |
| Variables should not be self-assigned | - |
| Variables should not be shadowed | suspicious, pitfall |

### Low Severity - Code Smells
| Rule | Tags |
|------|------|
| "Array.prototype.sort()" and "Array.prototype.toSorted()" should use a compare function | type-dependent, bad-practice |
| "Array.reduce()" calls should include an initial value | type-dependent |
| "await" should only be used with promises | type-dependent, confusing |
| "case" and "default" clauses should not contain lexical declarations | - |
| "catch" clauses should do more than rethrow | error-handling, unused |
| "continue" should not be used | bad-practice |
| "default" clauses should be last | - |
| "delete" should not be used on arrays | type-dependent |
| "for in" should not be used with iterables | type-dependent |
| "for of" should be used with Iterables | clumsy |
| "for" loop increment clauses should modify the loops' counters | confusing |
| "if ... else if" constructs should end with "else" clauses | - |
| "if" statements should be preferred over "switch" when simpler | bad-practice |
| "import" should be used to include external code | convention, es2015 |
| "indexOf" checks should not be for positive numbers | type-dependent, suspicious |
| "module" should not be used | obsolete |
| "RegExp.exec()" should be preferred over "String.match()" | regex, type-dependent |
| "shouldComponentUpdate" should not be defined when extending "React.PureComponent" | react |
| "strict" mode should be used with caution | cross-browser, user-experience |
| "switch" statements should have "default" clauses | cwe, type-dependent |
| "this" should not be assigned to variables | clumsy |
| "undefined" should not be passed as the value of optional parameters | type-dependent, confusing |
| **proto** property should not be used | type-dependent, confusing |
| A "while" loop should be used instead of a "for" loop | clumsy |
| A conditionally executed single line should be denoted by indentation | - |
| All "defaultProps" should have non-required PropTypes | react |
| All branches in a conditional structure should not have exactly the same implementation | - |
| All code should be reachable | cwe, unused |
| All enum members should be literals | enum |
| Allow confidential information to be logged is security-sensitive | privacy |
| Anchors should contain accessible content | accessibility, react |
| Anchor tags should not be used as buttons | accessibility, react |
| ARIA properties in DOM elements should have valid values | accessibility, react |
| Array constructors should not be used | suspicious |
| Array-mutating methods should not be used misleadingly | type-dependent |
| Assertion arguments should be passed in the correct order | chai, tests |
| Assignments should not be made from within sub-expressions | cwe, suspicious |
| Assignments should not be redundant | redundant |
| Boolean expressions should not be gratuitous | cwe, suspicious |
| Character classes in regular expressions should not contain only one character | regex, type-dependent |
| Character classes in regular expressions should not contain the same character twice | regex, type-dependent |
| Class names should comply with a naming convention | convention |
| Classes should not be empty | clumsy |
| Collection contents should be used | unused, suspicious |
| Comments should not be located at the end of lines of code | Deprecated |
| Comma and logical OR operators should not be used in switch cases | - |
| Comma operator should not be used | - |
| Default type parameters should be omitted | type-dependent, clumsy |
| Destructuring syntax should be used for assignments | es2015, clumsy |
| DOM elements should not use the "accesskey" property | accessibility, react |
| DOM elements should use the "autocomplete" attribute correctly | accessibility, react |
| DOM elements with ARIA role should only have supported properties | accessibility, react |
| DOM elements with ARIA roles should have a valid non-abstract role | accessibility, react |
| DOM elements with ARIA roles should have the required properties | accessibility, react |
| DOM elements with the `aria-activedescendant` property should be accessible via the tab key | accessibility, react |
| Elements with an interactive role should support focus | accessibility, react |
| Empty character classes should not be used | type-dependent |
| Empty collections should not be accessed or iterated | - |
| Ends of strings should be checked with "startsWith()" and "endsWith()" | type-dependent |
| Enum member values should be either all initialized or none | - |
| Enum members should not mix value types | type-dependent, enum |
| Enum values should be unique | - |
| Equality operators should not be used in "for" loop termination conditions | cwe, suspicious |
| Exceptions should not be ignored | cwe, error-handling |
| Extra semicolons should be removed | Deprecated |
| Fields that are only assigned in the constructor should be "readonly" | type-dependent, confusing |
| Files should not have too many lines of code | brain-overload, architecture |
| Focusable elements should not have "aria-hidden" attribute | accessibility, react |
| Function and method names should comply with a naming convention | convention |
| Function call arguments should not start on new lines | suspicious |
| Function parameters with default values should be last | es2015 |
| Function returns should not be invariant | - |
| Functions should not be empty | suspicious |
| Functions should not have identical implementations | suspicious, confusing |
| Functions should not have too many lines of code | brain-overload, architecture |
| Functions should not have too many parameters | brain-overload |
| Functions should use "return" consistently | api-design, confusing |
| Heading elements should have accessible content | accessibility, react |
| HTML "<table>" should not be used for layout purposes | accessibility, react |
| HTML elements should have a valid language attribute | accessibility, react |
| If statements should not be the only statement in else blocks | - |
| iFrames must have a title | accessibility, react |
| Image, area, button with image and object elements should have an alternative text | accessibility, react |
| Images should have a non-redundant alternate description | accessibility, react |
| Import variables should not be reassigned | bad-practice, confusing |
| Imports from the same module should be merged | es2015 |
| Imports should not use absolute paths | paths, pitfall |
| In React "this.state" should not be mutated directly | react |
| Interactive DOM elements should not have non-interactive ARIA roles | accessibility, react |
| Interfaces should not be empty | - |
| JavaScript parser failure | suspicious |
| JSX elements should not use unknown properties and attributes | react |
| JSX list components keys should match up between renders | performance, react |
| JSX list components should have a key property | performance, react |
| JSX list components should not use array indexes as key | performance, react |
| JSX special characters should be escaped | react |
| Label elements should have a text label and an associated control | accessibility, react |
| Labels should not be used | confusing |
| Lines should not be too long | Deprecated |
| Lines should not end with trailing whitespaces | Deprecated |
| Literals should not be thrown | error-handling, api-design |
| Literals should not be used for promise rejection | - |
| Local variables should not be declared and then immediately returned or thrown | clumsy |
| Loop counters should not be assigned within the loop body | - |
| Loops should not contain more than a single "break" or "continue" statement | brain-overload |
| Media elements should have captions | accessibility, react |
| Mergeable "if" statements should be combined | clumsy |
| Method overloads should be grouped together | convention |
| Methods should not contain selector parameters | design, type-dependent |
| Module should not import itself | architecture |
| Mouse events should have corresponding keyboard events | accessibility, react |
| Multiline blocks should be enclosed in curly braces | cwe |
| No ARIA role or property for unsupported DOM elements | accessibility, react |
| No redundant ARIA role | accessibility, react |
| Non-interactive DOM elements should not have an interactive handler | accessibility, react |
| Non-interactive DOM elements should not have interactive ARIA roles | accessibility, react |
| Non-interactive DOM elements should not have the `tabIndex` property | accessibility, react |
| Non-interactive elements shouldn't have event handlers | accessibility, react |
| Non-null assertions should not be used | unpredictable |
| Non-null assertions should not be used misleadingly | confusing, clumsy |
| Nullish coalescing should be preferred | type-dependent, nullish-coalescing |
| Object literal shorthand syntax should be used | convention, es2015 |
| Object spread syntax should be used instead of "Object.assign" | es2018 |
| Objects and classes converted or coerced to strings should define a "toString()" method | string, type-dependent |
| Octal escape sequences should not be used | - |
| Octal values should not be used | pitfall |
| Only "while", "do", "for" and "switch" statements should be labelled | pitfall |
| Optional boolean parameters should have default value | - |
| Optional chaining should be preferred | - |
| Optional property declarations should not use both '?' and 'undefined' syntax | type-dependent, redundant |
| Parameters should be passed in the correct order | type-dependent |
| Prefer tag over ARIA role | accessibility, react |
| Prefer the return type "this" in fluent interfaces | api-design, type-dependent |
| Primitive return types should be used | type-dependent, proficiency |
| Primitive types should be omitted from initialized or defaulted declarations | finding, clumsy |
| Property getters and setters should come in pairs | pitfall |
| Prototypes of builtin objects should not be modified | - |
| Public "static" fields should be read-only | cwe |
| Quotes for string literals should be used consistently | Deprecated |
| React "children" should not be passed as prop | react |
| React components should not be nested | performance, react |
| React Context Provider values should have stable identities | performance, react |
| React legacy lifecycle methods should not be used | react |
| React props should be read-only | type-dependent, react |
| React's "findDOMNode" should not be used | react |
| React's "isMounted" should not be used | react |
| Redundant type aliases should not be used | - |
| Regular expression literals should be used when possible | regex |
| Renaming import, export, and destructuring assignments should not be to the same name | - |
| Sections of code should not be commented out | unused |
| Shorthand object properties should be grouped at the beginning or end of an object declaration | convention, es2015 |
| Shorthand promises should be used | proficiency |
| Single-character alternations in regular expressions should be replaced with character classes | regex, type-dependent |
| Spacing between inline elements should be explicit | react |
| Special identifiers should not be bound or assigned | pitfall |
| Spread syntax should be used instead of "apply()" | - |
| Standard outputs should not be used directly to log anything | bad-practice, user-experience |
| Statements should be on separate lines | Deprecated |
| String literals should not be duplicated | design |
| String references should not be used | react |
| Strings and non-strings should not be added | type-dependent, confusing |
| Switch cases should end with an unconditional "break" statement | cwe, suspicious |
| Tabulation characters should not be used | Deprecated |
| Template literals should not be nested | brain-overload, confusing |
| Test files should contain at least one test case | node, tests |
| Tests should be stable | tests, unpredictable |
| Tests should check which exception is thrown | tests, chai |
| Tests should not execute any code after "done()" is called | tests, unpredictable |
| Tests should not be skipped without providing a reason | tests, bad-practice |
| The "any" type should not be used | pitfall |
| The return value of "ReactDOM.render" should not be used | react |
| The return value of "useState" should be destructured and named symmetrically | react |
| The ternary operator should not be used | brain-overload |
| Track comments matching a regular expression | Rule Template |
| Track uses of "FIXME" tags | cwe |
| Track uses of "TODO" tags | cwe |
| Trailing commas should be used | Deprecated |
| Two branches in a conditional structure should not have exactly the same implementation | design, suspicious |
| Type aliases should be used | proficiency |
| Unchanged variables should be marked as "const" | es2015 |
| Unnecessary calls to ".bind()" should not be used | - |
| Unnecessary character escapes should be removed | - |
| Unnecessary constructors should be removed | - |
| Unnecessary imports should be removed | es2015, type-dependent |
| Unused assignments should be removed | cwe, unused |
| Unused function parameters should be removed | unused |
| Unused private class members should be removed | unused |
| Unused React typed props should be removed | react |
| User-defined JSX components should use Pascal case | react |
| Users should not use internal APIs | gradle, architecture |
| Variable, property and parameter names should comply with a naming convention | convention |
| Variables should be used in the blocks where they are declared | pitfall |
| Variables should not be self-assigned | - |
| Variables should not be shadowed | suspicious, pitfall |

---

## Security Hotspots Summary

### Critical Security Areas
1. **Authentication & Session Management**
   - Creating public APIs
   - Hard-coded passwords/secrets
   - Session management during authentication

2. **Cryptography**
   - Weak cipher algorithms
   - Cryptographic key robustness
   - Encryption mode and padding schemes

3. **Data Protection**
   - Auto-escaping in templates
   - Cross-origin communication verification
   - SSL/TLS certificate and hostname verification

4. **Cloud Security (AWS)**
   - Public ACLs/policies on S3 buckets
   - Unencrypted resources (EBS, EFS, RDS, etc.)
   - IAM policy scope limitations

5. **Input Validation**
   - File upload restrictions
   - SQL injection prevention
   - XXE attack prevention

6. **Network Security**
   - CORS policies
   - Mixed content prevention
   - DNS prefetching control

---

## Quick Reference Categories

### Accessibility (a11y)
- ARIA roles and properties
- Keyboard navigation
- Screen reader support
- Focus management
- Alternative text for images
- Form labels
- Heading hierarchy
- Table headers

### Code Quality
- Complexity metrics (Cyclomatic, Cognitive)
- Code duplication
- Dead code elimination
- Naming conventions
- Code organization

### Performance
- Bundle size optimization
- Rendering optimization
- Memory leak prevention
- Efficient data structures

### Security
- XSS prevention
- CSRF protection
- SQL injection prevention
- Secure communication
- Data encryption
- Authentication/authorization

---

## Notes

- **Severity Levels**: Blocker > Critical > Major > Minor > Info
- **Issue Types**: Bug, Vulnerability, Code Smell, Security Hotspot
- **Tags**: Additional categorization for filtering and reporting
- **React-specific rules**: Tagged with `react`
- **Accessibility rules**: Tagged with `accessibility` and often `wcag2-a`
- **Security rules**: Tagged with `cwe` and specific vulnerability categories

---
