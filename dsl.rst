.. _dsl:

ClusterControl DSL
==================

This documentation provides detailed information on ClusterControl Domain Specific Language (DSL). The DSL syntax is similar to JavaScript, with extensions to provide access to ClusterControl’s internal data structures and functions. The DSL allows you to execute SQL statements, run shell commands/programs across all your cluster hosts, and retrieve results to be processed for advisors/alerts or any other actions.

Introduction
------------

This language will be provided for the users, to implement monitoring related code in a language similar to JavaScript. The code is executed on the Cmon Controller and will have access to the cluster nodes through the network.

The language is different from JavaScript in some ways. Here is a list of the most important differences:

* Semicolons are mandatory like in C or C++.
* Not all numbers are handled as double precision floating point values, we have integers and even usigned long long integers. We need those to handle disk sizes, network traffic.
* There are associative arrays with the data type ``Map``.
* The arrays are two dimensional, the one dimensional arrays has the type List although not many functions are using those.
* JavaScript uses a period in function names like ``JSON.parse(text)``, here we use the C++ notation like ``JSON::parse(text)``.
* New variables created on-the-fly in functions are local variables and not globals.
* The language implements a C like #include preprocessor directive.

Language basics
---------------

Preprocessor directives
````````````````````````

This system has no preprocessor, the parsing is done in one stage. Some expressions however are implemented the same way it is implemented in most of the C/C++ language preprocessors. Here is a list of the supported preprocessor directives.

* ``#include "filepath"``
	Preprocessor directive to include a source file the same way it is used for the C/C++ languages.

* ``#pragma once``
	Preprocessor directive to protect an include file to be included multiple times. The same as in the C/C++ languages.

Types
``````

Here is an example of the new operator.

.. code-block:: javascript

  var a = new Int;
  var b = new CmonHost();
  passed = 
    a === 0 &&
    b.typeName() == "CmonHost";

Types

* Int
* Bool
* Double
* Ulonglong
* String
* Error
* Map
* List
* Array
* CmonHost
* CmonMySqlHost
* CmonPostgreSqlHost
* CmonGaleraHost
* CmonAdvice
* CmonClusterConfig
* CmonGraph

Literals
````````

**Boolean literals**. These are just simply ``true`` and ``false``.

**String literals**. The string literals are handled the usual way, they can be enclosed in single or double quotes, single quoted strings may contain double quotes, the double quoted strings can contain single quotes.

.. code-block:: javascript

	var carName1 = "Volvo XC60";
	var carName2 = 'Volvo XC60';
	var answer1 = "It's alright";
	var answer2 = "He is called 'Johnny'";
	var answer3 = 'He is called "Johnny"';

Strings literals can also be concatenated in compile time as it is seen in the C/C++ languages:

.. code-block:: javascript

  var a = 
    "one "
    "two "
    "three";
  passed = a == "one two three";

**Integer literals.** Integer literals are integer numbers that are fit to be stored in the host computers "int" type. Here is an example:

.. code-block:: javascript

	var1 = 0xff;
	var2 = 0XFFFF;
	passed = var1 == 255 && var2 == 65535;

**Unsigned long long literals.** If an integer literal is too big to fit on an "int" type but fits in an unsigned long long it is automatically stored in an unsigned long long (or "Ulonglong" type). If the number is prefixed with "ull" it is also considered to have the Ulonglong type.

.. code-block:: javascript

  var a = 91872698761001;
  var b = 10ull;
  passed = 
    a.typeName() == "Ulonglong" &&
    b.typeName() == "Ulonglong";

**Double literals.** All the numbers that are not fit to any integer types will be stored in a double type as the usual double format strings.

.. code-block:: javascript
  var a = 10.2;
  var b = 10.8E11;
  var c = 2.8e-10;
  passed = 
    a.typeName() == "Double" &&
    b.typeName() == "Double" &&
    c.typeName() == "Double";

**Error literals.**

.. code-block:: javascript

  var a = #ARGS!;
  passed = a === #ARGS!;

The available error literals are the following:

* #NULL!
	Null value error.

* #DIV/0!
	Division by zero.

* #VALUE!
	Type mismatch error e.g. log of a string.

* #REF!
	Invalid variable reference, missing variable.

* #NAME?
	The name was not found, e.g. a function name is invalid.

* #NUM!
	Invalid numerical value e.g. sqrt(-1).

* #N/A
	Value is not available.

* #SYNTAX!
	Syntax error in formula.

* #ARGS!
	Argument number for a function is invalid.

**Map literals.** Map literals are associative arrays that can hold any type of values (even maps) and can be indexed by strings. Here is an example how to create and use a map:

.. code-block:: javascript

  var a = {};
  a["one"] = {};
  a["one"]["two"] = "value";
  passed = 
    a.typeName() == "Map" &&
    a["one"]["two"] == "value";

Functions
---------

If the function is called with the wrong number of arguments the return value will be an '#ARGS!' error (the type of the return value will be "Error").

Controller functions
````````````````````

* ``abort()``
	Aborts the execution of the script and presents the backtrace showing where exactly the abort() function was executed.

* ``exit(exitstatus)``
	Ends the execution of the script and returns the exitstatus to the Cmon environment. Ends the script with normal program termination.

* ``main(...)``
	If a main() function is provided once the program lines outside the functions are executed the main() function will be executed. The arguments of the main() function will be passed from the running environment and the return value of the main() will be sent back as exit status. Except if the exit() function is used to set the exit status.

.. code-block:: javascript

  var global1 = 10;
  function main(arg1)
  {
    return 
      arg1 == "UtCmonImperative" &&
      global1 === 10 &&
      global2 === 11;
  }
  var global2 = 11;

Input/Output functions
``````````````````````

* ``string print([value]...)``
	Prints all the vaues as one message with the severity set to 'info'. Also returns the printed string.

* ``string warning([value]...)``
	Prints all the vaues as one message with the severity set to 'warning'. Also returns the printed string.

* ``string error([value]...)``
	Prints all the vaues as one message with the severity set to 'critical'. Also returns the printed string.

General tag functions
`````````````````````

* ``string value.typeName()``
	Returns the type name of the value.

* ``string value.toString([formatid])``
	Returns the value converted to string. If the format ID is specified the string will be formatted accordingly. The available format specifiers are defined in "cmon/io.h".

.. code-block:: c++

  /*
   * Converting a double to string using various formats.
   */
  #include "cmon/io.h"
  
  var theDouble = 42.0;
  var str1      = theDouble.toString(TwoDecimalNumber);
  var str2      = theDouble.toString(FourDecimalNumber);
  var str3      = theDouble.toString(DateTime);
  
  passed = 
    str1 == "42.00" &&
    str2 == "42.0000" &&
    str3 == "Thu Jan  1 01:00:42 1970";

* ``boolean value.empty()``
	Returns true if the value is empty. The strings are empty when no characters are in them, the container objects (e.g. maps or lists) are empty when there is no items in them.

* ``int value.size()``
	The size of the strings is the number of characters in them, container objects hold the number of items as size. One mentionable exception is the Array objects that return the number of the columns as size, so it is easy to use them as single dimensional arrays (sometimes called vectors).

* ``boolean value.isNull()``
	Returns true if the value is a null string (e.g. an unset value from an SQL server).

* ``boolean value.isInvalid()``
	Returns true if the value is invalid, e.g. a variable with no value set before.

* ``boolean value.isString()``
	Returns true if the value is a string.

* ``int value.isInt()``
	Returns true if the value is an integer.

* ``boolean value.isULongLong()``
	Returns true if the value is an unsigned long long.

* ``boolean value.isDouble()``
	Returns true if the value is a double.

* ``boolean value.isBoolean()``
	Returns true if the value is a boolean.

* ``boolean value.isNumber()``
	Returns true if the type of the value is one of the number formats (e.g. int, ulonglong or double).

* ``boolean value.isError()``
	Returns true if the value is an error.

* ``boolean value.isMap()``
	Returns true if the value is a map.

* ``boolean value.isList()``
	Returns true if the value is a list.

* ``boolean value.isArray()``
	Returns true if the value is an array.

* ``int value.toInt()``
	Converts the value to the host computers integer format.

* ``ulonglong value.toULongLong()``
	Converts the value to unsigned long long.

* ``double value.toDouble()``
	Converts the value to double, strings with the usual number formats will be recognized.

* ``boolean value.toBoolean()``
	Converts the value into boolean. String like "true" and "false" will be recognized, integer values will have true value when they are not equal to zero.

Mathematical functions
``````````````````````

* ``number rand()``
	Creates a random number between 0 and 1.

* ``number pi()``
	Returns π, a mathematical constant, the ratio of a circle's circumference to its diameter.

* ``number degrees(number)``
	Converts radians to degrees.

* ``number radians(number)``
	Converts degrees to radians.

* ``number sign(number)``
	Returns -1 if the number is negative, +1 if not.

* ``number sin(number)``
	Returns the sine of an angle.

* ``number asin(number)``
	Returns the inverse hyperbolic sine of a number.

* ``number sinh(number)``
	Returns the hyperbolic sine of a number.

* ``number cos(number)``
	Returns the cosine of an angle.

* ``number acos(number)``
	Returns the inverse cosine of a number.

* ``number cosh(number)``
	Returns the hyperbolic cosine of a number.

* ``number acosh(number)``
	Returns the inverse hyperbolic cosine of a number.

* ``number fisher(number)``
	Returns the Fisher transformation of a number.

* ``number fisherinv(number)``
	Returns the inverse of the Fisher transformation of a number.

* ``number tan(number)``
	Returns the tangent of a number.

* ``number atan(number)``
	Returns the inverse tangent of a number.

* ``number tanh(number)``
	Returns the hyperbolic tangent of a number.

* ``number atanh(number)``
	Returns the inverse hyperbolic tangent of a number.

* ``number log(number, [base])``
	Returns the logarithm of a number to a specified base or to base 10 if the base is not specified.

* ``number sqrt(number)``
	Returns the square root of a number.

* ``number abs(number)``
	Returns the absolute value of number. Also works with arrays.

* ``number exp(number)``
	Returns e raised to the power of the given number.

* ``number floor(number, [significance])``
	Returns the number rounded down to the multiple of the given significance.

* ``number ceiling(number, [significance])``
	Returns the number rounded up to the multiple of the given significance.

* ``number round(number, digits)``
	Returns the number rounded to the given number of digits.

* ``number roundup(number, digits)``
	Returns the number rounded up to the given number of digits.

* ``number rounddown(number, digits)``
	Returns the number rounded down to the given number of digits.

* ``number mround(number, multiple)``
	Returns the number rounded to the given multiple.

* ``number even(number)``
	Returns the number rounded to the nearest even number.

* ``number iseven(number)``
	Returns true if the number is even.

* ``number odd(number)``
	Returns the number rounded to the nearest odd number.

* ``number convert(number, from, to)``
	Converts between units. Supported units are byte, kbyte, mbyte, gbyte, tbyte, celsius, kelvin, fahrenheit, hz, mhz, ghz. When the 'units' utility program is installed more units are available for CONVERT().

* ``number isodd(number)``
	Returns true if the number is odd.

Functions providing information about values
````````````````````````````````````````````

* ``boolean iserr(value)``
	Returns true if the value is an error.

* ``boolean isnumeric(value)``
	Returns true if the value is a number.

* ``boolean istext(value)``
	Returns true if the value is a string.

* ``boolean isnumber(value)``
	Returns true if the value is a number.

* ``boolean isarray(value)``
	Returns true if the value is an array.

String functions
````````````````

* ``integer asc(text)``
	Returns the ASCII value of the first character in the string.

* ``text char(number)``
	Returns the character that has the given number as ASCII value.

* ``text chr(number)``
	Returns the character that has the given number as ASCII value.

* ``text left(text, number)``
	Returns the leftmost characters of string.

* ``text right(text, number)``
	Returns the rightmost characters of a string.

* ``boolean startswith(text1, text2)``
	Returns true if text1 starts with text2.

* ``boolean endwith(text1, text2)``
	Returns true if text1 ends with text2.

* ``text mid(text, start, length)``
	Returns the substring of a text that starts at the given location has at most the given length.

* ``text escape(text)``
	Returns the text escaped with backslash characters.

* ``text unescape(text)``
	Returns the text after the escaping characters has been removed.

* ``text upper(text)``
	Returns the text converted to uppercase.

* ``text lower(text)``
	Returns the text converted to lowercase.

* ``text trim(text)``
	Returns the text after removing the white characters from the beginning and the end.

* ``number len(text)``
	Returns the length of a string.

* ``text concatenate(text, [text]...)``
	Returns a string that hols all the arguments concatenated.

* ``number int(value)``
	Returns the value converted into an integer number.

* ``number cbool(value)``
	Returns the value converted into a logical (boolean) value.

* ``number cstr(value)``
	Returns the value converted into a string.

* ``number cdbl(value)``
	Returns the value converted into a floating point double precision number.

String tag functions
````````````````````

* ``int string.length()``
	Returns the length of the string.

* ``int string.indexOf(substring, [start])``
	Returns the position of the first occurrence of the substring in the string. Retturns -1 of the substring was not found.

* ``array string.split(separatorstring)``
	Returns an array that contains all the substrings separated by the given separator in the original string.

* ``string string.substr(begin, length)``
	Extracts parts of a string, beginning at the character at the specified position. Returns the specified number of characters.

* ``string string.trim()``
	Returns the string without the leading and tailing whitespace characters.

* ``string string.toLowerCase()``
	Returns the string converted to lower case letters.

* ``string string.toUpperCase()``
	Returns the string converted to upper case characters.

* ``boolean string.contains(string)``
	Returns true if the string contains the argument as substring.

* ``string string.replace(string, string)``
	Returns the string that has the given substring replaced to the second argument.

* ``boolean string.looksInteger()``
	Returns true if the string represents an integer number that can be stored as an Int type value.

* ``integer string.toInteger()``
	Converts the string to an Int type integer number.

* ``boolean string.looksULongLong()``
	Returns true if the string converts to an integer fits on an Ulonglong but will not fit on an Int type.

* ``boolean string.looksDouble()``
	Returns true if the string can be converted to a Double type number.

* ``boolean string.looksBoolean()``
	Returns true if the string is a textual representation of a boolean value (e.g. "true" or "false").

* ``integer string.toULongLong()``
	Converts the string to an Ulonglong type integer number.

* ``boolean string.looksEmail()``
	Returns true if the string represents a valid e-mail address.

* ``boolean string.looksIpAddress()``
	Returns true if the string represents a valid IPv4 address.

General array functions
````````````````````````

* ``value choose(position, value, [value]...)``
	Returns the value at the given position of the values. The first value is returned when position is 1.

* ``array transpose(array)``
	Returns the transposed version of an array where rows are converted to columns and columns are converted to rows.

* ``array filterrows(array, column, value)``
	Returns an array that contains only those rows matching to a specific value in the specified column.

* ``number columns(array)``
	Returns how many columns the array has.

* ``number rows(array)``
	Returns how many rows the array has.

* ``value vlookup(value, array, column, [notExact])``
	Performs a vertical lookup in the leftmost column of the array and returns the value of the found row from an other column.

* ``value hlookup(value, array, column, [notExact])``
	Performs a horizontal lookup in the leftmost column of the array and returns the value of the found column from an other row.

* ``value match(value, array, [matchType])``
	Searches for a value and returns the relative position of the item found.

Statistical functions
``````````````````````

Statistical functions will provide statistical calculations on number sets. The values for most functions can be passed through individual arguments or using arrays. Here is an example:

.. code-block:: javascript

  b = [ 10, 8, 5 ];
  c = average(b);
  d = average(10, 11, 12);

* ``number count(value, [value]...)``
	Returns how many of the values and array elements contains a number.

* ``number countblank(value, [value]...)``
	Returns how many of the values and array elements are empty.

* ``number min(value, [value]...)``
	Returns the smallest number.

* ``number max(value, [value]...)``
	Returns the largest number.

* ``number sum(value, [value]...)``
	Returns the sum of all number.

* ``number sumsq(value, [value]...)``
	Returns the sum of the squares of the numbers.

* ``number product(value, [value]...)``
	Returns the product of all the numbers.

* ``number average(arg1, [arg2]...)``
	Calculates the average, the arithmetic mean value for a set of numbers or arrays.

* ``number geomean(arg1, [arg2]...)``
	Calculates the geometric mean of a set of positive numbers.

* ``number mode(value1, [value2]...)``
	Returns the most frequently occurring value of a data set.

* ``number emaverage(alpha, value1, [value2]...)``
	Calculates the exponential moving average.

* ``number median(value1, [value2]...)``
	Returns the median of the numbers.

* ``number percentile(array, [n])``
	Returns the nth percentile of the numbers in the array.

* ``number small(array, n)``
	Returns the nth smallest number of an array.

* ``number large(array, n)``
	Returns the nth largest number of an array.

* ``number stdev(value1, [value2]...)``
	Returns the standard deviation of a sample.

* ``number avedev(value1, [value2]...)``
	Returns the average of the absolute deviations of the number.

* ``number pearson(array1, array2)``
	Returns the Pearson product-moment correlation coefficient between two sets of numbers.

* ``number correl(array1, array2)``
	Returns the correlation coefficient of the array1 and array2.

* ``number covar(array1, array2)``
	Returns the average of the products of deviations for each data pair.

* ``number devsq(value1, [value2]...)``
	Returns the sum of the squares of deviations from the average.

* ``number var(value1, [value2]...)``
	Returns the variance of a set of numbers.

* ``number forecast(x, knownYValues, knownXValues)``
	Estimates future values from existing data using the linear regression method.

* ``number linest(knownYValues, knownXValues)``
	Uses the "least squares" method to find the linear equation that fits the data. Returns an array with the slope and height of the equation.

* ``number slope(knownYValues, knownXValues)``
	Uses the "least squares" method to find the linear equation that fits the data. Returns the slope of the equation.

* ``number intercept(knownYValues, knownXValues)``
	Uses the "least squares" method to find the linear equation that fits the data. Returns the y-axis intersection point of the line.

CmonDateTime functions
``````````````````````

This type is different from the JavaScript Date type.

* ``CmonDateTime CmonDateTime::currentDateTime()``
	Returns the real-time clock time from the host computer (controller).

* ``CmonDateTime CmonDateTime::fromUnixTime(time)``
	Converts the unix time (seconds elapsed from epoch) a CmonDateTime value.

* ``CmonDateTime CmonDateTime::fromString(string)``
	Converts the string to a date&time format value. The recognized string formats are somewhat limited.

* ``int CmonDateTime::timeZone()``
	Returns how many seconds must be added to the local time to get UTC (Coordinated Universal Time). CET for example is 1 hour ahead of UTC and so the return value is -3600.

* ``int CmonDateTime::dayLight()``
	Returns how many seconds must be added to the local time because of the daylight saving time

* ``CmonDateTime dateTime::toString([format])``
	Converts the date&time to string. The available formats are defined in the "cmon/io.h" header file. Here are some examples:

.. code-block:: c++

  #include "cmon/io.h"
  
  var dateTime = CmonDateTime::fromUnixTime(1424686003);
  
  str01 = dateTime.toString(FileNameFormat);
  str02 = dateTime.toString(ShortDayFormat);
  str03 = dateTime.toString(LogFileFormat);
  str04 = dateTime.toString(MySqlLogFileFormat);
  str05 = dateTime.toString(MySqlShortLogFormat);
  str05 = dateTime.toString(MySqlLogFileDateFormat);
  str06 = dateTime.toString(MySqlShortLogDateFormat);
  str07 = dateTime.toString(ShortTimeFormat);
  str08 = dateTime.toString(LongTimeFormat);
  str09 = dateTime.toString(ShortDateFormat);
  str10 = dateTime.toString(LocalDateTimeFormat);
  str11 = dateTime.toString(EmailDateTimeFormat);
  
  passed = 
    str01 == "2015-02-23_110643" &&
    str02 == "150223" &&
    str03 == "Feb 23 11:06:43" &&
    str04 == "2015-02-23 11:06:43" &&
    str05 == "2015-02-23" &&
    str06 == "150223" &&
    str07 == "11:06" &&
    str08 == "11:06:43" &&
    str09 == "02/23/15" &&
    str10 == "Mon Feb 23 11:06:43 2015" &&
    str11 == "Mon, 23 Feb 2015 11:06:43 +0100";

* ``int dateTime.second()``
	Returns the 'seconds' part of the time.

* ``int dateTime.minute()``
	Returns the 'minute' part of the time.

* ``int dateTime.hour()``
	Returns the 'hours' part of the time.

* ``int dateTime.hour()``
	Returns the 'hours' part of the time.

* ``int dateTime.month()``
	Returns the month in the year between 1 and 12.

* ``int dateTime.year()``
	Returns the year of the date like 2014.

* ``int dateTime.weekday()``
	Sunday = 1, Monday = 2,... Saturday = 7.

CmonHost tag functions
``````````````````````

Here is an example for the CmonHost tag functions. The variable host1 here has the CmonHost object type.

.. code-block:: javascript

  hosts       = cluster::hosts();
  host1       = hosts[0];
  
  hostName    = host1.hostName();
  port        = host1.port();

* ``string CmonHost::hostname()``
	Returns the name of the host as it was provided by the user. If the Cmon configuration file for example holds the host name as IP address this function will return the hostname as a string representation of that address.

* ``int CmonHost::port()``
	Returns the port number of the host. This is usually the port number of the SQL server.

* ``int CmonHost::clusterId()``
	Returns the cluster ID of the cluster of the host. The Cluster ID is a unique ID number Cmon uses to identify the cluster.

* ``string CmonHost::ipAddress()``
	Returns the string that holds the IPv4 address of the host.

* ``boolean CmonHost::connected()``
	FIXME: Documentation.

* ``string CmonHost::message()``
	Returns a human readable string that describes the status of the host.

* ``string CmonHost::description()``
	FIXME: Documentation.

* ``string CmonHost::distributionName()``
	Returns the name of the OS distribution running on the host.

* ``string CmonHost::distributionCodeName()``
	Returns the code name of the OS distribution running on the host.

* ``string CmonHost::distributionRelease()``
	Returns the release number of the OS distribution running on the host.

* ``string CmonHost::role()``
	FIXME: Documentation.

* ``int CmonHost::pingDelay()``
	FIXME: Documentation.

* ``string CmonHost::serverVersion()``
	For hosts running used as SQL servers returns the version number of the SQL server software, for a controller returns the Cmon software version number.

* ``string CmonHost::toJSonString()``
	Converts the host to a JSON string.

* ``map CmonHost::toMap()``
	Converts the host object to a map where all properties of the host are held and accessible with string keys. Convert the host to JSON message to see what properties are available and what keys are used.

* ``string host.checkValue(type, value)``
	Checks the given value according the rules of the given alarm type, activates an alarm related to the given host or clears the alarm acordingly. Please check the `Alarms`_ section for examples.

* ``string host.raiseAlarm(type, severity, [message])``
	Activates an alarm related to the given host. Please check the `Alarms`_ section for examples.

* ``string host.clearAlarm(type)``
	Clears an alarm related to the given host. Please check the `Alarms`_ section for examples.

* ``list host.alarms()``
	Returns all the active alarms related to the given host. Please check the `Alarms`_ section for examples.

* ``map host.memoryInfo()``
	Returns a map with the latest memory information statistics of the host. The fields in the map are described in the CmonMemoryStats properties section. If the requested data is not collected for some reason the #N/A error is returned.

* ``list host.memoryStats(startTime, endTime)``
	Returns a list with the memory information statistics of the host. The fields in the map are described in the CmonMemoryStats properties section. If the requested data is not collected for some reason the #N/A error is returned. See the `Obtaining and processing statistical information`_ section for some examples.

* ``map host.sqlInfo()``
	Returns a map with the latest sql server statistics of the host. The fields in the map are described in the CmonSqlStats properties section. If the requested data is not collected for some reason the #N/A error is returned.

* ``list host.sqlStats(startTime, endTime)``
	Returns a list with the sql server statistics of the host. The fields in the map are described in the CmonSqlStats properties section. If the requested data is not collected for some reason the #N/A error is returned. See the `Obtaining and processing statistical information`_ section for some examples.

* ``list host.networkInfo()``
	Returns a list of maps, one list item for each monitored network interface. The fields in the map are documented in the CmonNetworkStats properties section.

* ``list host.diskInfo()``
	Returns a list of maps, one list item for each monitored disk partition. The fields in the maps are documented in the CmonDiskStat properties section.

* ``list host.diskStats(startTime, endTime, [devicename])``
	Returns a list of maps, one list item for each disk stats sample. If the third option is provided returns only samples from that device. The fields in the maps are documented in the CmonDiskStat properties section.

* ``list host.cpuInfo()``
	Returns a list of maps, one list item for each CPU cores. The fields in the maps are documented in the CmonCpuStats properties section.

* ``list host.cpuStats(startTime, endTime, [coreid])``
	Returns a list of maps, one list item for each CPU statistical sample in the given periiod. If the third argument is provided returns only samples for the cpu with the given ID. The fields in the maps are documented in the CmonCpuStats properties section.

* ``map host.system(command)``
	Executes a shell command on the host. Returns a map that contains information about the return value and the standard output of the executed process.

.. code-block:: javascript

  function main()
  {
    var hosts     = cluster::hosts();
    var host      = hosts[0];
    var retval;
    retval = host.system("ls -lha /home");
    if (!retval["success"])
    {
      error("ERROR: ", retval["errorMessage"]);
    }
    print("Result: ", retval["result"]);
    return retval["success"];
  }

* ``CmonClusterConfig host.config([fileName])``
	Loads the configuration from the host (together with the include files and the files from the include directories) and returns a CmonClusterConfig object that holds all the information from the files.

* ``map host.executeSqlQuery(query)``
	Executes the SQL query (an SQL expression that has return data, e.g. a SELECT) and returns the results. The returned map will have a value for "success" to show if the operation was successful or not, an "errorMessage" that holds a human readable error message and a "result" field that holds an array with all the data the SQL server sent. Please check `Executing SQL commands`_ and queries section for some examples. A similar function (``CmonDb::executeSqlQuery()``) is available to execute an SQL query on the Cmon Database.

* ``map host.executeSqlCommand(sqlCommand)``
	Executes the SQL command (an SQL expression that has no return data, e.g. an INSERT or an UPDATE) and returns the status. The returned map will have a value for "success" to show if the operation was successful or not and an "errorMessage" that holds a human readable error message. Please check `Executing SQL commands and queries`_ section for some examples.

* ``boolean host.sqlPing([timeout])``
	Executes a neutral SQL command (e.g. SELECT 1;) on the host to see if the SQL server up and able to run queries. Returns true if the SQL server returns a valid reply. If the argument is provided it controls how many seconds the the function will try to reach the server.

* ``CmonMySqlHost tag functions``
	The CmonMySqlHost inherits all the properties and tag functions of the CmonHost.

* ``boolean host.isGalera()``
	Returns true if the MySQL host is a Galera host.

* ``ulonglong host.uptime()``
	Returns the 'uptime' status variable that shows the SQL server uptime in seconds.

* ``boolean host.readOnly()``
	Returns the value of the 'read_only' SQL variable.

CmonClusterConfig tag functions
````````````````````````````````

The CmonClusterConfig is a class that represents a set of configuration files found on one or more hosts of the cluster.

* ``string config.errorMessage()``
	Returns a human readable error message that descibe the state of the last operation.

* ``list config.variable([variableName])``
	Returns a list of variables found in the configuration. If the variable name is not provided returns all the variables defined in the configuration file. Every list element is a Map that holds the following keys: "variablename", "linenumber", "value", "filepath" and "section".

* ``value config.setVariable(section, variableName, value)``
	Sets the variable in the given section to the given value. If the variable or the section is not in the configuration it will be added. Please note that this function schanges the configuration object, the change to has an effect the cluster configuration has to be saved.

* ``map config.save()``
	Saves the configuration to the original host(s) using the original filename(s). The return map shall have the "success" and the "errorMessage" set to reflect if the operation was successful.

CmonAdvice tag functions
````````````````````````

CmonAdvice is a class that represents an action to be taken by the administrator of the cluster advised by the advisor, a code that executed by the Cmon Controller. An advice is mostly constructed of human readable edscriptions together with some information that help tracking where and when the advice was created.

* ``void advice.setTitle(title)``
	Sets the title for the advice. The title should be a short description for the advice.

* ``string advice.title()``
	Returns the title of the advice.

* ``void advice.setCreator(creator)``
	Sets the name of the owner that created the advice. This is automatically set to the source file.

* ``string advice.creator()``
	Returns the name of the creator.

* ``void advice.setJustification(justification)``
	Sets the justification for the advice. The justificatio is a detailed description about the reason why the advisor decided there should be an action taken. The justification usually contain measured values if human readable form.

* ``string advice.justification()``
	Returns the justification for the advice.

* ``void advice.setAdvice(string)``
	Sets the detailed description of the advice.

* ``string advice.advice()``
	Returns the detailed description of the advice.

* ``void advice.setSeverity(Severity)``
	Sets the severity level for the advice. Severity level for an avice is the same as the severity levels for the alarms as it is defined in alarms.h.

* ``Severity advice.severity()``
	Returns the severity level for the advice.

* ``void advice.setHost(host)``
	Sets the host for the advice so the user will know which host was investigated when the advice was given.

CmonGraph tag functions
````````````````````````

A CmonGraph consists of one or more plots. These plots are usually shown as lines, lines with points on them or bars to represent a number of values in a two dimensional (x/y) coordinate system. Various properties of these plots can be set using the plot index, that is a number referencing the plots from 1 to the last plot.

* ``void advice.setTitle(title)``
	Sets the text that is shown on the top of the graph image.

* ``boolean graph.setSize(width, height)``
	Sets the size of the generated graph in pixels.

* ``boolean graph.setXDataIsTime([boolean])``
	Sets if the X axis values should be printed as date and time value.

* ``boolean graph.setPlotLegend(plotIdx, legend)``
	Sets the text that is shown for the given plot as legend.

* ``boolean graph.setPlotColumn(plotIdx, xColumn, yColumn)``
	Sets which data array column is used as data for the given plot.

* ``boolean graph.setPlotStyle(plotIdx, style)``
	Sets what style will be used to plot the data. Check the "cmon/graph.h" include file for the available styles.

Cluster functions
``````````````````

* ``array cluster::hosts()``
	Returns all the CmonHosts that are considered as part of he cluster. This function also returns the host of the Cmon controller.

* ``array cluster::mySqlNodes()``
	Returns all the CmonMySqlHosts that are considered as part of the cluster. The CmonMySqlHost inherits the properties and functions of the CmonHost, so where a CmonHost can be used a CmonMySqlHost is also accepted (e.g. CmonHost::executeSqlQuery() also works for CmonMySqlHost).

* ``array cluster::galeraNodes()``
	Returns the Galera nodes of the cluster. The returned list holds CmonGaleraHost type items.

* ``array cluster::postgreSqlNodes()``
	Returns the MySQL nodes of the cluster. The returned list holds CmonMySqlHost type items.

* ``array cluster::mongoNodes()``
	Returns the MongoDb nodes of the cluster. The returned list holds CmonMongoNode type items.

* ``array cluster::ndbdNodes()``
	Returns the Ndb nodes of the cluster. The returned list holds CmonNdbHost type items.

* ``string cluster::statustext()``
	Returns the human readable description of the cluster status.

* ``int cluster::state()``
	FIXME: documentation.

* ``bool cluster::rollingRestart()``
	Restarts the nodes without stopping the cluster.

Cmon functions
``````````````

* ``text cmon::version()``
	Returns the Cmon version as a string.

* ``text cmon::build()``
	Returns the Cmon build number as a string.

* ``number cmon::uptime()``
	Returns how many seconds ago Cmon started to manage this cluster.

* ``boolean cmon::running()``
	Returns true if Cmon is managing this cluster.

* ``text cmon::hostname()``
	Returns the host name of the computer running the Cmon controller.

* ``text cmon::domainname()``
	Returns the domain name of the computer running the Cmon controller.

General alarm functions
```````````````````````

* ``int Alarm::alarmId(category, isHost, title, message, recommendation)``
	Registers a new alarm type if an alarm type with the same properties is not registered already. Returns the alarm type that can be used to raise and clear alarms with these properties. Use this function to implement custom alarms for the Cmon system. Please check the `Alarms`_ section for examples.

* ``int Alarm::checkId(category, isHost, warningLevel, criticalLevel, title, message, recommendation)``
	Registers a new check type if a check type with the same properties is not registered already. Checks are in reality alarms that have warning and critical levels so they can easily be used to check numerical values. Returns the alarm type that can be used to raise and clear alarms with these properties calling the ``host.checkValue()`` function.

Mail functions
``````````````

* ``Map Mail::sendMail(subject, body, [component])``
	Appends a new mail message to the outgoing folder of the Cmon system. The third (optional) argument controls the component which will ultimately used to decide what recipients will get the email. The possible values are defined in the ComponentType enum in the "cmon/alarms.h" file.

License functions
``````````````````

* ``text license::statustext()``
	Returns a short string describing the status of the license.

* ``bool license::status()``
	Returns true if there is a valid license for the cluster.

* ``int license::expires()``
	A negative value indicates the Cmon license expired or not found, positive values show how many days the license has left.

Cmon Database functions
````````````````````````

The Cmon Database is the SQL database where the Cmon stores all its internal data. This database is accessible from the JS programs.

* ``Map CmonDb::executeSqlQuery(query)``
	Executes the SQL query on the Cmon Database and returns the results. The returned map will have values for the keys "success", "errorMessage" and "result" where the value for teh ley "result" is a two dimensional array that holds the values from the SQL query.

Examples
--------

Executing SQL commands and queries
``````````````````````````````````

The following example demonstrates how to execute an SQL query on an arbitrary host of a cluster and receive the results in an array. The return value of the ``host.executeSqlQuery()`` holds the success/failed status of the query, the error message and also the results in a two dimensional array.

.. code-block:: c++

  function getSqlVariable(host, variableName)
  {
    var query = "SHOW GLOBAL STATUS LIKE '$1'";
    var retval;
    var value;
    if (host.typeName() != "CmonHost")
      return #ARGS!;
    query.replace("$1", variableName);
    retval = host.executeSqlQuery(query);
    if (!retval["success"])
    {
      print("ERROR:", retval["errorMessage"]);
      return #N/A;
    }
    value = retval["result"][0, 1];
    if (value.looksInteger())
      return value.toInt();
    else if (value.looksULongLong())
      return value.toULongLong();
    else if (value.looksDouble())
      return value.toDouble();
    return value;
  }
  function main()
  {
    var hosts = cluster::hosts();
    var value = getSqlVariable(hosts[0], "COM_SELECT");
    print("*** value: ", value);
    return value.isInt();
  }

Here is an example that shows how to execute an SQL query on the Cmon Database:

.. code-block:: c++

  var retval = CmonDb::executeSqlQuery("select * from mysql_states;");
  var passed = true;
  if (!retval["success"])
  {
    error("Executing SQL query failed: ", retval["errorMessage"]);
  }
  
  for (idx = 0; idx < retval["result"].rows(); ++idx)
  {
    var string = retval["result"][idx, 2];
    print(retval["result"][idx, 2]);
    if (string.empty())
    {
      error("Value at idx = ", idx, " is empty.");
    }
  }

Executing shell commands
`````````````````````````

.. code-block:: javascript

  function listFiles(host)
  {
    var retval = host.system("ls -lha /home");
    if (!retval["success"])
      error("ERROR: ", retval["errorMessage"]);
    print("Host    : ", host.hostName());
    print("Result  : ", retval["result"]);
    print("Success : ", retval["success"]);
    return retval["success"];
  }

Obtaining and processing statistical information
`````````````````````````````````````````````````

The following example shows how to obtain statistical data from a specific host about a specific time interval and how to process the data using low-level indexing operators. For the most task there are more efficient high-level statistical functions that can be used without looping through the data, but low-level access can be also beneficial for custom calculations.

.. code-block:: c++

  //
  // Going through the memory statistics of the last 10 minutes and printing the
  // size of the free memory with the time.
  //
  #include "cmon/io.h"
  function toGigaBytes(value)
  {
    return value / (1024 * 1024 * 1024);
  }
  function main()
  {
    var host      = cluster::hosts()[0];
    var endTime   = CmonDateTime::currentDateTime();
    var startTime = endTime - 10 * 60;
    var stats     = host.memoryStats(startTime, endTime);
    var retval    = true;
    for (idx = 0; idx < stats.size(); ++idx)
    {
      map     = stats[idx];
      created = CmonDateTime::fromUnixTime(map["created"]);
      ramfree = toGigaBytes(map["ramfree"]);
      print(
        created.toString(LongTimeFormat), 
        " ", ramfree.toString(TwoDecimalNumber), "GBytes");
    }
    return retval;
  }

The next example shows a sophisticated recipe to process some of the statistical data using high level statistical functions. It first get the data calling the ``host.memoryStats()`` function, then it filters all the memory utilization information into an array. This array then can be processed by statistical functions like ``min()``, ``max()`` or ``percentile()``.

.. code-block:: c++

  //
  // Printing the min, the ninth percentile and the max of the memory utilization
  // in the last ten minutes for every host. Prints something like this:
  //
  //        MEMORY UTILIZATION 
  // HOST       MIN     NINTH     MAX
  // 127.0.0.1 42.00% - 42.58% - 42.64%
  //
  #include "cmon/io.h"
  function printUtil(host, startTime, endTime)
  {
    var list  = host.memoryStats(startTime, endTime);
    var array = list.toArray("memoryutilization");
    var min   = min(array);
    var max   = max(array);
    var ninth = percentile(array, 0.9);
    print(host.hostName(),
          " ", 
          min.toString(TwoDecimalPercent), " - ",
          ninth.toString(TwoDecimalPercent), " - ",
          max.toString(TwoDecimalPercent));
    
    return true;
  }
  function main()
  {
    var endTime   = CmonDateTime::currentDateTime();
    var startTime = endTime - 10 * 60;
    var hosts     = cluster::hosts();
  
    print("       MEMORY UTILIZATION ");
    print("HOST       MIN     NINTH     MAX");
    for (idx = 0; idx < hosts.size(); ++idx)
      printUtil(hosts[idx], startTime, endTime);
    return true;
  }

Alarms
``````

.. code-block:: c++

  #include "cmon/alarms.h"
  var hosts   = cluster::hosts();
  var host    = hosts[0];
  var alarms;
  var found   = false;
  //
  // Raising an alarm
  //
  host.raiseAlarm(MySqlAdvisor, Critical, "Some message.");
  //
  // Reading the active alarms and searching for the same alarm.
  //
  alarms = host.alarms();
  for (idx = 0; idx < alarms.size(); ++idx)
  {
    if (alarms[idx]["title"] == "MySQL advisor alarm")
    {
      found = true;
      break;
    }
  }

The following example demonstrates how to create a custom alarm type and raise an alarm with the custom alarm.

.. code-block:: c++

  //
  // Demonstating custom alarms.
  //
  #include "cmon/alarms.h"
  //
  // This function returns an alarm type for a custom alarm with some 
  // properties encoded into the function.
  //
  function myAlarm()
  {
    return Alarm::alarmId(
          Node, true, 
          "Computer is on fire", 
          "The computer is on fire, it is on flames.", 
          "Pour some water on it.");
  }
  var myAlarmId = myAlarm();
  var hosts     = cluster::hosts();
  var host      = hosts[0];
  var sentMessage;
  sentMessage = host.raiseAlarm(myAlarmId, Critical);
  //
  // An alarm is raised and sentMessage should be:
  // Server 127.0.0.1 reports: The computer is on fire, it is on flames.
  //

Configuration files
````````````````````

.. code-block:: c++

  function getConfiguredClientPort(host)
  {
    var config      = host.config();
    var variable    = config.variable("port");
    for (idx = 0; idx < variable.size(); ++idx)
    {
      print("*** section  : ", variable[idx]["section"]);
      print("*** value    : ", variable[idx]["value"]);
      print("*** location : ", 
            variable[idx]["filepath"], ":", variable[idx]["linenumber"]);
      if (variable[idx]["section"] == "client")
          return variable[idx]["value"].toInt();
    }
    return #N/A;
  }

Creating graphs
````````````````

The following example shows how to create a graph, set up with statistical data and return to the UI to be shown as an image.

.. code-block:: c++

  //
  // This is a test program that prints a graph on the sql statistics.
  //
  #include "cmon/graph.h"
  var hosts     = cluster::hosts();
  var host      = hosts[0];
  var endTime   = CmonDateTime::currentDateTime();
  var startTime = endTime - 10 * 60;
  var stats     = host.sqlStats(startTime, endTime);
  var array     = stats.toArray(
    "created,interval,COM_SELECT,COM_INSERT");
  //
  // Calculating some values from the statistics
  //
  for (idx = 0; idx < array.columns(); idx++)
  {
    array[5, idx] = 1000 * array[2, idx] / array[1, idx];
    array[6, idx] = 1000 * array[3, idx] / array[1, idx];
  }
  var graph     = new CmonGraph;
  graph.setXDataIsTime();
  graph.setTitle("SQL Statistics " + host.toString());
  graph.setSize(800, 600);
  // This graph contains two plots, we set the various properties for them here 
  // here. The plot index will be 1 and 2.
  graph.setPlotLegend(1, "Select (1/s)");
  graph.setPlotColumn(1, 0, 5);
  graph.setPlotStyle(1, Impulses);
  graph.setPlotLegend(2, "Insert (1/s)");
  graph.setPlotColumn(2, 0, 6);
  graph.setPlotStyle(2, Impulses);
  graph.setData(array);
  exit(graph);

To-do
-----

* The switch and case are not implemented.
* The typeof x is not implemented, although we have the x.typeName() for the same purpose.
* The obj1 = {a: 1} notation is not implemented.
* The with is not implemented.
* Passing function object arguments as references is not implemented.
* Implement a CmonTimer based solution to measure milliseconds.
* Functions inside functions are not implemented.
* bash host {...}

This looks promising... somewhat beneficial...

.. code-block:: javascript

  var myStructure = {
    name: {
      first: "Mel",
      last: "Smith"
    },
    age: 33,
    hobbies: ["chess", "jogging"]
  };

Variadic functions would also be nice to have:

.. code-block:: javascript

  var sum = function() {
    var i, x = 0;
    for (i = 0; i < arguments.length; ++i) {
      x += arguments[i];
    }
    return x;
  }
  sum(1, 2, 3); // returns 6
