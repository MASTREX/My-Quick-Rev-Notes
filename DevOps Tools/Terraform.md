- Use variable names as dynamic keys in map, use **parantheses**
  ```terraform
  custom_tags = {
	(local.environment_tag_name)   = local.environment_tag
	(local.applicatio_id_tag_name) = local.app_id
	CostCentre                     = local.cost_centre
  }
  ```

## Functions

- `length(list | map | string)`
- `join(separator, list)`
- `split(separator, string)`
- `format(specs, values...)`: specs in ***printf*** style
- `range(start, excluded-limit, step)`

  - range(limit): range(3) = [0, 1, 2]
  - range(start, limit): range(2, 7) = [2, 3, 4, 5, 6]
  - range(start, limit, step): range(4, 7, 2) = [4, 6]

- `merge(map1, map2)`: Combines multiple maps. If keys overlap, the value from the **last map** is used.
- `compact(list)`: Removes **null** or **empty strings** ("") from a list.
- `coalesce(args...)`: Returns the **first non-null/non-empty** argument. Useful for fallbacks.
- `lookup(map, key, default)`: Retrieves a value from a map. If the key is missing, it returns the `default` instead of an error.
- `setproduct(list1, list2)`: Returns a list of all possible combinations (Cartesian product) of the input sets.
- `try(expr, fallback)`: Evaluates an expression and returns a fallback if it errors.
