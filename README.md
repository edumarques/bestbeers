# Brain teaser
## Find the best beers in the tavern!
### Scenario:
Once upon a time there was a tavern with 1000 beer taps, numbered from 1 to 1000. You were told by a mysterious stranger that the best beers are the ones with the taps whose numbers match these 2 conditions:

- The sum of the divisors (including 1, but not the number itself) of the tap number is greater than tap number itself.
- No subset of those divisors sums up to the tap number itself.
The waiter is coming, what is your order?

For example:

- Number 12: the proper divisors are 1, 2, 3, 4 and 6. The sum is 1+2+3+4+6 = 16 which is greater than 12 and matches the first condition. However, the subset 2+4+6=12 which violates the second condition.


#### Result: 70, 836


#### Solution:

~~~
// --- FIND THE BEST BEERS IN THE TAVERN (ANALOG TO WEIRD NUMBERS PROBLEM) ---

// define amount of numbers
var amountOfBeers = 1000;
// define array to store the best beers
var bestBeers = [];
// loop over all numbers
for (var number = 1; number < amountOfBeers; number++) {
	// check if number is one of the best and store it in the array
    if (isBestBeer(number)) {
        bestBeers.push(number);
    }
}

// define function to check if best beer
function isBestBeer (number) {
    // set first divisor 1 (common to all numbers)
    var divisors = [1];
    // initialize sum of divisors
	var sum = 0;
    //set limit to restrict search and improve efficiency 
	var max = number/2;
    // loop over array of divisors, initialize with 2 since we already have 1 in the array 
	for(var potentialDivisor = 2; potentialDivisor <= max; potentialDivisor++ ) {
		// check if potential divisor is in fact a divisor
        if(number%potentialDivisor==0) {
            // check if divisor is not already in the array
			if(divisors.indexOf(potentialDivisor) == -1) {
                // push divisor into array
				divisors.push(potentialDivisor);
                // add up divisor to sum
				sum += potentialDivisor;
			}
            // change limit since new divisor was just found
			max = number/potentialDivisor;
            // check if divisor is not already in the array
			if(divisors.indexOf(max) == -1) {
                // push divisor into array
				divisors.push(max);
                // add up divisor to sum
				sum += max;
			}
		}
	}
    // check first condition (the sum of the number's divisors is greater than the number itself)
	if(sum > number) { 
        // sort divisors in descending order to improve efficiency when finding the sum of every subset and checking second condition
		divisors.sort(function(a, b) {
            return b - a;
        }); 
        // check second condition (no subset of number's divisors sums up to the number itself)
		if(!isSubsetSumEqualToNumber(divisors,divisors.length,number)) { 
            return true;
		} else {
            return false;
        }
	} else {
        return false;
    } 
}

// define function to check if sum of subsets equals number
function isSubsetSumEqualToNumber(divisors, amountOfDivisors, number)  { 
	// if number is 0, its only subset [] sums up to 0
    if (number == 0) {
        return true;
    }
    // when the amount of divisor reaches 0, that means no subset of divisors is equal to the number
	if (amountOfDivisors == 0) {
        return false;
    }

    // compares smallest divisor to number, if greater return this function but without divisor just used for comparison
	if (divisors[amountOfDivisors - 1] > number) {
        return isSubsetSumEqualToNumber(divisors, amountOfDivisors - 1, number);
    }

    // - when smallest divisor is less than number, return this function without this just used smallest divisor
    // and also return this function with a new number (number - current smallest divisor)
    // - do logic expression to check both returns
    // -- p.s.: here is where it's defined whether there is any subset of the number equal to the number itself (at least one case returns true)
    // or no subset equals the number (both cases return false)
	return isSubsetSumEqualToNumber(divisors, amountOfDivisors - 1, number) ||
            isSubsetSumEqualToNumber(divisors, amountOfDivisors - 1, number - divisors[amountOfDivisors - 1]); 
}

~~~
