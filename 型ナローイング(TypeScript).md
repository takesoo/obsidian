---
tags:
  - TypeScript
aliases:
  - 型ナローイング
  - Type Narrowing
---
```ts
function padLeft(padding: number | string, input: string): string {
	if (typeof padding === "number") {
		// ここではpaddingはnumber型になる
		return " ".repeat(padding) + input;
	}
	return padding + input;
}
```

```ts
// value is string | numberによって、valueはunknown型から string | number 型に推論される
function isStringOrNumber(value: unknown): value is string | number {
	return typeof value === "string" || typeof value === "number";
}

const something: unknown = 123;

if (isStringOrNumber(something)) {
	// ここではsomethingは string | number 型
	conosole.log(something.toString());
}

```