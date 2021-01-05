# Calculator from a string


## Question
You need to pass an string and it should even validate parenthesis.

E.g:

```
(2-0)(6/2) = 6
```

It should evaluate priority. Parenthesis => Division => Multiplying => Any other operation

## Solution

```js
function Calculator(str) { 
  for (let charIndex in str) {
    if (str[charIndex] === '(') {      
      for (let endParenthesisIndex = parseInt(charIndex) + 1; endParenthesisIndex <= str.length; endParenthesisIndex++) {
        if (str[endParenthesisIndex] === ')') {
          const endSign = str[endParenthesisIndex + 1] === "(" ? "*" : '';
          const operation = str.slice(parseInt(charIndex) + 1, endParenthesisIndex);
          const result = Calculator(operation)
          str = str.replace(`(${operation})`, `${result}${endSign}`);
          break;
        }
      }
    }
  }

  str = OperationSolve(str, '/')
  str = OperationSolve(str, '*')
  str = OperationSolve(str, '-')
  str = OperationSolve(str, '+')

  return str; 
}

function OperationSolve(str, operator) {
  for (let charIndex in str) {
    if (str[charIndex] === operator) {
      const leftNumberResult = FindLeftNumbers(str, parseInt(charIndex))
      const rightNumberResult = FindRightNumbers(str, parseInt(charIndex))
      
      let result = 0;

      switch (operator) {
        case '/':
          result = parseInt(leftNumberResult.number) / parseInt(rightNumberResult.number); 
          break;
        case '*':
          result = parseInt(leftNumberResult.number) * parseInt(rightNumberResult.number); 
          break;
        case '-':
          result = parseInt(leftNumberResult.number) - parseInt(rightNumberResult.number); 
          break;
        case '+':
          result = parseInt(leftNumberResult.number) + parseInt(rightNumberResult.number); 
          break;
      }

      str = str.replace(`${leftNumberResult.number}${operator}${rightNumberResult.number}`, result)
    }
  }

  return str;
}

function FindLeftNumbers(operation, operatorIndex) {
  let leftNumberDigits = []
  for (let leftIndex = operatorIndex - 1; leftIndex >= 0; leftIndex--) {
    if (Number.isNaN(parseInt(operation[leftIndex]))) {
      break;
    }

    leftNumberDigits.unshift(parseInt(operation[leftIndex]));
  }

  return { number: leftNumberDigits.join(''), indexToRemove: leftNumberDigits.length }
}

function FindRightNumbers(operation, operatorIndex) {
  let rightNumberDigits = []
  for (let rightIndex = operatorIndex + 1; rightIndex <= operation.length; rightIndex++) {
    if (Number.isNaN(parseInt(operation[rightIndex]))) {
      break;
    }

    rightNumberDigits.push(parseInt(operation[rightIndex]));
  }

  return { number: rightNumberDigits.join(''), indexToRemove: rightNumberDigits.length }
}
   
// keep this function call here 
console.log(Calculator(readline()));
```