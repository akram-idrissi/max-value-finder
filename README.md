# Max Finder (PHP)

**Challenge:**  
Write a function in PHP that finds the maximum integer inside a possibly nested array of arrays.  
Only `count()` and `is_array()` from the standard library are allowed.

## Function

```php
<?php

declare(strict_types=1);

/**
 * Recursively find the maximum integer value in a nested array.
 *
 * Only integers and arrays are expected in the input.
 * Uses only `count()` and `is_array()` from the standard library.
 *
 * @param array<int|array> $xs The input array which may contain integers or nested arrays.
 *
 * @return int The maximum integer found within the nested structure.
 *
 * @throws InvalidArgumentException If the array (or nested arrays) are empty.
 */
function my_max(array $xs): int
{
    if (count($xs) === 0) {
        throw new InvalidArgumentException('Cannot determine max of an empty array.');
    }

    $max = PHP_INT_MIN;

    for ($i = 0; $i < count($xs); $i++) {
        $value = $xs[$i];

        if (is_array($value)) {
            if (count($value) === 0) {
                continue;
            }
            $nestedMax = my_max($value);
            if ($nestedMax > $max) {
                $max = $nestedMax;
            }
        } else {
            if ($value > $max) {
                $max = $value;
            }
        }
    }

    if ($max === PHP_INT_MIN) {
        throw new InvalidArgumentException('All nested arrays were empty.');
    }

    return $max;
}

echo my_max([1, [2, 3]]) . PHP_EOL;                 // 3
echo my_max([5, [1, 10, [20, -3]], 7]) . PHP_EOL;   // 20
echo my_max([-5, [-10, -3], -7]) . PHP_EOL;         // -3

// echo my_max([]);                // InvalidArgumentException
// echo my_max([[], []]);          // InvalidArgumentException
