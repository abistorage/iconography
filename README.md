### 2. Multiple constants - Treeshakable and typesafe with one file (`conversionType==='constants'`)

This approach converts your svg icons into multiple constants in the same file so that they can be used
in combination with an icon registry. It furthermore also generates all necssary types. **We wrote a step to step guide that explains this approach further and helps you create an icon library with this approach.**
[Find out more in this blogpost](https://medium.com/angular-in-depth/how-to-create-an-icon-library-in-angular-4f8863d95a)

![Output scenario one](https://raw.githubusercontent.com/kreuzerk/svg-to-ts/master/assets/example-src1.png)
Only the icons included in the consuming SPA also end up in the final bundle of the SPA.

#### Available options:

| --version          | type                       | default                                  | description                                                                           |
| ------------------ | -------------------------- | ---------------------------------------- | ------------------------------------------------------------------------------------- |
| typeName           | string                     | myIcons                                  | name of the generated type                                                            |
| generateType       | boolean                    | false                                    | prevent generating enumeration type                                                   |
| generateTypeObject | boolean                    | false                                    | generate type object                                                                  |
| generateEnum       | boolean                    | false                                    | generate enum object                                                                  |
| prefix             | string                     | myIcon                                   | prefix for the generated svg constants                                                |
| interfaceName      | string                     | MyIcon                                   | name for the generated interface                                                      |
| fileName           | string                     | my-icons                                 | file name of the generated file                                                       |
| enumName           | string                     | MyIcons                                  | name for the generated enum                                                           |
| delimiter          | CAMEL, KEBAB, SNAKE, UPPER | SNAKE                                    | delimiter which is used to generate the types and name properties                     |
| svgoConfig         | string or config object    | check help command - to large to display | a path to your svgoConfiguration JSON file or an inline configuration object          |
| srcFiles           | string                     | "/\*.svg"                                | input files matching the given filename pattern                                       |
| outputDirectory    | string                     | "./dist"                                 | name of the output directory                                                          |
| verbose            | boolean                    | false                                    | defines if the log should contain additional information. Can be useful for debugging |

#### Example usage

Let's say we have the following four svg files in a `inputfiles` folder.

- expressionless.svg
- full.svg
- laughing.svg
- smiling-face.svg

We can now run
`svg-to-ts.ts --conversionType constants -s ./inputfiles -o ./dist`
and we end up with the following file in our `dist` folder.

#### Sample ouput

```javascript
export const myIconExpressionLess: MyIcon = {
  name: 'expression_less',
  data: `<svg xmlns="http://...`
};
export const myIconFull: MyIcon = {
  name: 'full',
  data: `<svg xmlns="http://www...`
};
export const myIconLaughing: MyIcon = {
  name: 'laughing',
  data: `<svg xmlns="http://www.w...`
};
export const myIconSmilingFace: MyIcon = {
  name: 'smiling_face',
  data: `<svg xmlns="http://www.w3...`
};
/* ⚠️ Do not edit this file - this file is generated by svg-to-ts*/

export type myIcons = 'expression_less' | 'full' | 'laughing' | 'smiling_face';
export interface MyIcon {
  name: myIcons;
  data: string;
}
```

### 3. Tree shakable and optimized for lazy loading (`conversionType==='files'`)

This is the most sophisticated approach and also the approach that doesn't only support tree shaking but also
supports code splitting which is especially usefull in scenarios where you are using lazy loading.

[Here's a step by step guide on how to create an icon library that is optimized for tree shaking](https://medium.com/angular-in-depth/how-to-create-a-fully-tree-shakable-icon-library-in-angular-c5488cf9cd76)

![fully tree shakable](https://raw.githubusercontent.com/kreuzerk/svg-to-ts/master/assets/fully-treeshakable.png)
Often, having the SVGs in a single file is enough. However, if you are in a more complex environment with bigger business
applications, you may want to make the icons even more tree shakable.

In Angular, for example, having all icons in a single file shakes out the icons that are not used. However, icons always
end up together in a chunk. The `conversionOption = files` allows you to configure `svg-to-ts` that icons are
generated in a way that they can even be split to lazy loaded chunks. Means not only the amount of the icons in the chunk
gets reduced, but also, where they end up. Means, an icon that is only used in a lazy loaded Angular feature module, will only
end up there.

#### Available options:

| --version                 | type                       | default                                  | description                                                                                                                                                                     |
| ------------------------- | -------------------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| barrelFileName            | string                     | index                                    | name of the generated type                                                                                                                                                      |
| typeName                  | string                     | myIcons                                  | name of the generated type                                                                                                                                                      |
| generateType              | boolean                    | false                                    | prevent generating enumeration type                                                                                                                                             |
| generateTypeObject        | boolean                    | false                                    | generate type object                                                                                                                                                            |
| generateEnum              | boolean                    | false                                    | generate enum object                                                                                                                                                            |
| exportCompleteIconSet     | boolean                    | false                                    | Specifies if the complete icon set should be exported or not (can be very handy for showcases)                                                                                  |
| prefix                    | string                     | myIcon                                   | prefix for the generated svg constants                                                                                                                                          |
| interfaceName             | string                     | MyIcon                                   | name for the generated interface                                                                                                                                                |
| modelFileName             | string                     | my-icons                                 | file name of the generated file                                                                                                                                                 |
| enumName                  | string                     | MyIcons                                  | name for the generated enum                                                                                                                                                     |
| delimiter                 | CAMEL, KEBAB, SNAKE, UPPER | SNAKE                                    | delimiter which is used to generate the types and name properties                                                                                                               |
| srcFiles                  | string                     | "/\*.svg"                                | input files matching the given filename pattern                                                                                                                                 |
| svgoConfig                | null or config object      | check help command - to large to display | by default we search for a svgo.config.js file in the root or an inline configuration object                                                                                    |
| outputDirectory           | string                     | "./dist"                                 | name of the output directory                                                                                                                                                    |
| additionalModelOutputPath | string                     | null                                     | if a path is specified we will generate an additional file containing interface and type to this path - can be useful to improve type safety                                    |
| iconsFolderName           | string                     | "build"                                  | name of the folder we will build the TypeScript files to                                                                                                                        |
| compileSources            | boolean                    | false                                    | If set to false, we generate a TypeScript file for each SVG. If set to true we will allready compile those TypeScript files and generate JavaScript files and declaration files |
| verbose                   | boolean                    | false                                    | defines if the log should contain additional information. Can be useful for debugging                                                                                           |
