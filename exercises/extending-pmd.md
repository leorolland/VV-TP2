# Extending PMD

Use XPath to define a new rule for PMD to prevent complex code. The rule should detect the use of three or more nested `if` statements in Java programs so it can detect patterns like the following:

```Java
if (...) {
    ...
    if (...) {
        ...
        if (...) {
            ....
        }
    }

}
```
Notice that the nested `if`s may not be direct children of the outer `if`s. They may be written, for example, inside a `for` loop or any other statement.
Write below the XML definition of your rule.

You can find more information on extending PMD in the following link: https://pmd.github.io/latest/pmd_userdocs_extending_writing_rules_intro.html

Use your rule with different projects and describe you findings below. See the [instructions](../sujet.md) for suggestions on the projects to use.

## Answer

### XPath basic knowledge (from https://stackoverflow.com/questions/8998402/how-to-have-nested-conditions-for-pmd-xpath-rules)
[] is called predicate. It must contain a boolean expression. It must immediately follow a node-test. This specifies an additional condition for a node that satisfies the node-test to be selected.

```xml
  <rule   name="TooManyNestedIfs" 
	  language="java" 
	  message="Violation : Too many nestes Ifs control structures"
	  class="net.sourceforge.pmd.lang.rule.XPathRule">
  	<description>There are 3+ nested if control structures.</description>
	<priority>3</priority>
	<properties>
		<property name="xpath">
			<value>
			//IfStatement[count(ancestor::IfStatement) = 2]
			</value>
		</property>
	</properties>
  </rule>

```

Then run the PMD and filter results with the following command : 
`pmd -d . -R pmd-ruleset.xml 2>/dev/null| grep "TooManyNestedIfs"`

which gives us the output (sample)
```
/home/black/cours/VV/commons-math/src/test/java/org/apache/commons/math4/util/FastMathTest.java:561:	TooManyNestedIfs:	Violation : Too many nestes Ifs control structures
/home/black/cours/VV/commons-math/src/test/java/org/apache/commons/math4/util/FastMathTest.java:581:	TooManyNestedIfs:	Violation : Too many nestes Ifs control structures
/home/black/cours/VV/commons-math/src/test/java/org/apache/commons/math4/util/FastMathTest.java:594:	TooManyNestedIfs:	Violation : Too many nestes Ifs control structures
/home/black/cours/VV/commons-math/src/test/maxima/special/RealFunctionValidation/RealFunctionValidation.java:108:	TooManyNestedIfs:	Violation : Too many nestes Ifs control structures
/home/black/cours/VV/commons-math/src/test/maxima/special/RealFunctionValidation/RealFunctionValidation.java:129:	TooManyNestedIfs:	Violation : Too many nestes Ifs control structures
/home/black/cours/VV/commons-math/src/test/maxima/special/RealFunctionValidation/RealFunctionValidation.java:241:	TooManyNestedIfs:	Violation : Too many nestes Ifs control structures
/home/black/cours/VV/commons-math/src/userguide/java/org/apache/commons/math4/userguide/genetics/Polygon.java:97:	TooManyNestedIfs:	Violation : Too many nestes Ifs control structures
```

Let's count the number of violations by adding `| wc -l` pipe :
```sh
➜  commons-math git:(master) ✗ pmd -d . -R pmd-ruleset.xml 2>/dev/null| grep "TooManyNestedIfs" | wc -l
206
```

There are 206 violations about too many nested ifs.
