# q-image

## Usage

#### Install

```
yarn install @qneyraud/q-image
```

#### Properties

| name      | type   | required | default         | description                                                                                                                                                                             |
| --------- | ------ | -------- | --------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| alt       | String | yes      | ""              | Alternative text                                                                                                                                                                        |
| source       | String | yes      | ""              | Default image source                                                                                                                                                                    |
| jpg       | Array  | no       | null            | List of object containing jpg `source` and `width` properties                                                                                                                           |
| webp      | Array  | no       | null            | List of object containing webp `source` and `width` properties                                                                                                                          |
| sizes     | String | no       | null            | Image tag `sizes` attribute                                                                                                                                                             |
| placement | String | no       | "cover"         | `object-fit` css rule                                                                                                                                                                   |
| position  | String | no       | "center center" | `object-position` css rule                                                                                                                                                              |
| color     | String | no       | null            | Component background color. You will see it before image lazyloading or if placement is 'contain'                                                                                       |
| lazyload  | String | no       | "none"          | `none` to disable lazyloading <br> `direct` to lazyload image after page load <br> `called` to wait for a `lazyloadImage()` method call <br> `intersection` to use IntersectionObserver |
| threshold | Number | no       | 0.0             | IntersectionObserver threshold                                                                                                                                                          |

#### Methods

| name          | return | description                 |
| ------------- | ------ | --------------------------- |
| lazyloadImage | void   | Start lazyloading the image |

#### Events

| name   | parameters                 | description                      |
| ------ | -------------------------- | -------------------------------- |
| loaded | reference to the component | Emitted when the image is loaded |


## Development

#### Serve

```
yarn run serve
```

### build

```
yarn run build-bundle
```
