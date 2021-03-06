<template>
  <DoricText
    v-if="type === 'Text' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricImage
    v-else-if="type === 'Image' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricVLayout
    v-else-if="type === 'VLayout' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricHLayout
    v-else-if="type === 'HLayout' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricStack
    v-else-if="type === 'Stack' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricRoot
    v-else-if="type === 'Root' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricScroller
    v-else-if="type === 'Scroller' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricSwitch
    v-else-if="type === 'Switch' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricInput
    v-else-if="type === 'Input' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricFlexLayout
    v-else-if="type === 'FlexLayout' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricSlider
    v-else-if="type === 'Slider' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricSlideItem
    v-else-if="type === 'SlideItem' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricList
    v-else-if="type === 'List' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <DoricListItem
    v-else-if="type === 'ListItem' && !(doricModel.nativeViewModel.props.hidden === true)"
    :doricModelProps="doricModel"
    @tap.native.stop="onclick"
  />
  <view v-else>
    <text v-if="type !== null && !(doricModel.nativeViewModel.props.hidden === true)"
      >Error: {{ type }}</text
    ></view
  >
</template>

<script lang="ts">
import { Vue, Component, Prop, Watch } from 'vue-property-decorator'

import {
  Align,
  GradientColor,
  GradientOrientation,
  IAnimation,
  LayoutSpec,
  NativeViewModel,
  TranslationAnimation,
  View,
} from 'doric'
import { toPixelString, toRGBAString, DoricModel } from '../../../doric/utils'
import { callResponse } from '../../../doric/context'

import DoricText from '../../../doric/shader/doric-text/index.vue'
import DoricImage from '../../../doric/shader/doric-image/index.vue'
import DoricVLayout from '../../../doric/shader/doric-vlayout/index.vue'
import DoricHLayout from '../../../doric/shader/doric-hlayout/index.vue'
import DoricStack from '../../../doric/shader/doric-stack/index.vue'
import DoricRoot from '../../../doric/shader/doric-root/index.vue'
import DoricScroller from '../../../doric/shader/doric-scroller/index.vue'
import DoricSwitch from '../../../doric/shader/doric-switch/index.vue'
import DoricInput from '../../../doric/shader/doric-input/index.vue'
import DoricFlexLayout from '../../../doric/shader/doric-flexlayout/index.vue'
import DoricSlider from '../../../doric/shader/doric-slider/index.vue'
import DoricSlideItem from '../../../doric/shader/doric-slideitem/index.vue'
import DoricList from '../../../doric/shader/doric-list/index.vue'
import DoricListItem from '../../../doric/shader/doric-listitem/index.vue'
import { doricInterface } from '../../../doric/interface'

@Component({
  name: 'DoricNode',
  components: {
    DoricText,
    DoricImage,
    DoricHLayout,
    DoricVLayout,
    DoricStack,
    DoricRoot,
    DoricScroller,
    DoricSwitch,
    DoricInput,
    DoricFlexLayout,
    DoricSlider,
    DoricSlideItem,
    DoricList,
    DoricListItem,
  },
})
export default class extends Vue {
  @Prop() private doricModelProps!: any

  doricModel: unknown = null
  type: unknown = null

  @Watch('doricModelProps', { immediate: true })
  onDoricModelPropsChange(newVal: DoricModel) {
    if (!newVal) return

    const childrenViewIdsLength =
      (newVal?.nativeViewModel?.props?.children as string[])?.length || 0
    const subviewsLength =
      (newVal?.nativeViewModel?.props?.subviews as NativeViewModel[])?.length || 0
    if (childrenViewIdsLength != subviewsLength) {
      let type = newVal.nativeViewModel.type
      if (type == 'HLayout' || type == 'VLayout' || type == 'Root' || type == 'Stack') {
        return
      }
    }

    // impl code
    const doricModel = newVal
    const props = doricModel.nativeViewModel.props as Partial<View>
    const doricStyle: Record<string, string> = doricModel.cssStyle

    if (props.layoutConfig) {
      doricStyle['margin-left'] = toPixelString(props.layoutConfig.margin?.left || 0)
      doricStyle['margin-right'] = toPixelString(props.layoutConfig.margin?.right || 0)
      doricStyle['margin-top'] = toPixelString(props.layoutConfig.margin?.top || 0)
      doricStyle['margin-bottom'] = toPixelString(props.layoutConfig?.margin?.bottom || 0)
    }
    if (props.border) {
      doricStyle['border-style'] = 'solid'
      doricStyle['border-width'] = toPixelString(props.border.width)
      doricStyle['border-color'] = toRGBAString(props.border.color as unknown as number)
    }
    if (props.padding) {
      doricStyle['padding-left'] = toPixelString(props.padding.left || 0)
      doricStyle['padding-right'] = toPixelString(props.padding.right || 0)
      doricStyle['padding-top'] = toPixelString(props.padding.top || 0)
      doricStyle['padding-bottom'] = toPixelString(props.padding.bottom || 0)
    }
    let width: string
    const widthSpec = props.layoutConfig?.widthSpec || LayoutSpec.JUST
    switch (widthSpec) {
      case LayoutSpec.FIT:
        width = 'max-content'
        break
      case LayoutSpec.MOST:
        if (props.padding) {
          width = `calc(100 - ${props.padding.left}px - ${props.padding.right}px)`
        } else {
          width = '100%'
        }

        break
      case LayoutSpec.JUST:
      default:
        width = toPixelString(
          (props.width || 0) -
            (props.padding?.left || 0) -
            (props.padding?.right || 0) -
            (props.border?.width || 0) * 2,
        )
        break
    }
    doricStyle['width'] = width
    let height: string
    const heightSpec = props.layoutConfig?.heightSpec || LayoutSpec.JUST
    switch (heightSpec) {
      case LayoutSpec.FIT:
        height = 'max-content'
        break
      case LayoutSpec.MOST:
        if (props.padding) {
          height = `calc(100 - ${props.padding.top}px - ${props.padding.bottom}px)`
        } else {
          height = '100%'
        }
        break
      case LayoutSpec.JUST:
      default:
        height = toPixelString(
          (props.height || 0) -
            (props.padding?.top || 0) -
            (props.padding?.bottom || 0) -
            (props.border?.width || 0) * 2,
        )
        break
    }
    doricStyle['height'] = height

    if (props.backgroundColor) {
      if (typeof props.backgroundColor === 'number') {
        doricStyle['background-color'] = toRGBAString(props.backgroundColor as unknown as number)
      } else if (typeof props.backgroundColor === 'object') {
        let gradient = props.backgroundColor as GradientColor

        let deg = ''
        switch (gradient.orientation) {
          case GradientOrientation.TOP_BOTTOM:
            deg = '180deg'
            break
          case GradientOrientation.TR_BL:
            deg = '-135deg'
            break
          case GradientOrientation.RIGHT_LEFT:
            deg = '-90deg'
            break
          case GradientOrientation.BR_TL:
            deg = '-45deg'
            break
          case GradientOrientation.BOTTOM_TOP:
            deg = '0deg'
            break
          case GradientOrientation.BL_TR:
            deg = '45deg'
            break
          case GradientOrientation.LEFT_RIGHT:
            deg = '90deg'
            break
          case GradientOrientation.TL_BR:
            deg = '135deg'
            break
        }

        if (gradient.start && gradient.end) {
          doricStyle['background-image'] = `linear-gradient(${deg}, ${toRGBAString(
            gradient.start as unknown as number,
          )}, ${toRGBAString(gradient.end as unknown as number)});`
        } else {
          if (gradient.locations) {
            if (gradient.colors) {
              const colors = gradient.colors
                .map(
                  (e, index) =>
                    `${toRGBAString(e as unknown as number)} ${
                      ((gradient.locations!![index] as unknown as number) * 100).toFixed(2) + '%'
                    }`,
                )
                .join(',')
              doricStyle['background-image'] = `linear-gradient(${deg}, ${colors});`
            }
          } else {
            if (gradient.colors) {
              const colors = gradient.colors
                .map(e => `${toRGBAString(e as unknown as number)}`)
                .join(',')
              doricStyle['background-image'] = `linear-gradient(${deg}, ${colors});`
            }
          }
        }
      }
    }

    if (props.x) {
      doricStyle['left'] = toPixelString(props.x)
    }
    if (props.y) {
      doricStyle['top'] = toPixelString(props.y)
    }

    if (props.corners) {
      if (typeof props.corners === 'number') {
        doricStyle['border-radius'] = toPixelString(props.corners as number)
      } else if (typeof props.corners === 'object') {
        let corners = props.corners as {
          leftTop: number
          rightTop: number
          leftBottom: number
          rightBottom: number
        }
        doricStyle['border-top-left-radius'] = toPixelString(corners.leftTop)
        doricStyle['border-top-right-radius'] = toPixelString(corners.rightTop)
        doricStyle['border-bottom-left-radius'] = toPixelString(corners.leftBottom)
        doricStyle['border-bottom-right-radius'] = toPixelString(corners.rightBottom)
      }
    }

    if (props.shadow) {
      const opacity = props.shadow.opacity || 0
      let boxShadow
      if (opacity > 0) {
        const offsetX = props.shadow.offsetX || 0
        const offsetY = props.shadow.offsetY || 0
        const shadowColor = props.shadow.color || 0xff000000
        const shadowRadius = props.shadow.radius
        const alpha = opacity * 255
        boxShadow = `${toPixelString(offsetX)} ${toPixelString(offsetY)} ${toPixelString(
          shadowRadius,
        )} ${toRGBAString(((<any>shadowColor) & 0xffffff) | ((alpha & 0xff) << 24))} `
      } else {
        boxShadow = ''
      }
      doricStyle['box-shadow'] = boxShadow
    }

    if (props.alpha !== undefined) {
      doricStyle['opacity'] = props.alpha.toString()
    }

    if (props.flexConfig) {
      const flexConfig = props.flexConfig

      if (flexConfig.alignContent) {
        if ((flexConfig.alignContent as number) == Align.AUTO) {
          doricStyle['align-content'] = 'unset'
        } else if ((flexConfig.alignContent as number) == Align.FLEX_START) {
          doricStyle['align-content'] = 'flex-start'
        } else if ((flexConfig.alignContent as number) == Align.CENTER) {
          doricStyle['align-content'] = 'center'
        } else if ((flexConfig.alignContent as number) == Align.FLEX_END) {
          doricStyle['align-content'] = 'flex-end'
        } else if ((flexConfig.alignContent as number) == Align.STRETCH) {
          doricStyle['align-content'] = 'stretch'
        } else if ((flexConfig.alignContent as number) == Align.BASELINE) {
          doricStyle['align-content'] = 'baseline'
        } else if ((flexConfig.alignContent as number) == Align.SPACE_BETWEEN) {
          doricStyle['align-content'] = 'space-between'
        } else if ((flexConfig.alignContent as number) == Align.SPACE_AROUND) {
          doricStyle['align-content'] = 'space-around'
        }
      }

      if (flexConfig.marginLeft) {
        const type = Object.getPrototypeOf(flexConfig.marginLeft)
        if (type == Number.prototype) {
          doricStyle['margin-left'] = toPixelString((flexConfig.marginLeft as number) || 0)
        }
      }
      if (flexConfig.marginRight) {
        const type = Object.getPrototypeOf(flexConfig.marginRight)
        if (type == Number.prototype) {
          doricStyle['margin-right'] = toPixelString((flexConfig.marginRight as number) || 0)
        }
      }
      if (flexConfig.marginTop) {
        const type = Object.getPrototypeOf(flexConfig.marginTop)
        if (type == Number.prototype) {
          doricStyle['margin-top'] = toPixelString((flexConfig.marginTop as number) || 0)
        }
      }
      if (flexConfig.marginBottom) {
        const type = Object.getPrototypeOf(flexConfig.marginBottom)
        if (type == Number.prototype) {
          doricStyle['margin-bottom'] = toPixelString((flexConfig.marginBottom as number) || 0)
        }
      }
    }

    doricModel.cssStyle = doricStyle
    this.doricModel = doricModel
    this.type = doricModel.nativeViewModel.type
  }

  onclick(event: Event) {
    const doricModel = this.doricModelProps as DoricModel
    const props = doricModel.nativeViewModel.props as Partial<View>
    if ((doricModel.idList, props.onClick)) {
      callResponse(doricModel.contextId, doricModel.idList, props.onClick as unknown as string)
    }
  }

  async getWidth() {
    if (this.$children && (this.$children as any[]).length > 0) {
      let result = await (this.$children as any[])[0].computeSize()
      return result.width
    } else {
      return 0
    }
  }

  async getHeight() {
    if (this.$children && (this.$children as any[]).length > 0) {
      let result = await (this.$children as any[])[0].computeSize()
      return result.height
    } else {
      return 0
    }
  }

  async getX() {
    if (this.$children && (this.$children as any[]).length > 0) {
      let result = await (this.$children as any[])[0].computeSize()
      return parseFloat(result['margin-left'].replace('px', ''))
    } else {
      return 0
    }
  }

  async getY() {
    if (this.$children && (this.$children as any[]).length > 0) {
      let result = await (this.$children as any[])[0].computeSize()
      return parseFloat(result['margin-top'].replace('px', ''))
    } else {
      return 0
    }
  }

  async getLocationOnScreen() {
    if (this.$children && (this.$children as any[]).length > 0) {
      let result = await (this.$children as any[])[0].computeSize()
      return {
        x: result.left,
        y: result.top,
      }
    } else {
      return 0
    }
  }

  async doAnimation(args: TranslationAnimation) {
    const changeables = args.changeables
    const repeatCount = args.repeatCount
    const repeatMode = args.repeatMode
    const fillMode = args.fillMode
    const timingFunction = args.timingFunction
    console.log(args)
    doricInterface.createAnimation({})
  }
}
</script>

<style>
.doric-node {
}
</style>
