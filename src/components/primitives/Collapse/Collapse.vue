<script>
import { ref, h } from 'vue'

const Collapse = {
  name: 'VCollapse',
  props: {
    open: {
      type: Boolean,
      default: true
    },
    animation: {
      type: String,
      default: 'fade'
    },
    ariaId: {
      type: String,
      default: ''
    },
    position: {
      type: String,
      default: 'is-top'
    }
  },
  render(props) {
    const isOpen = ref(props.open)

    // TODO Upgrade from createElement to h musb be done 

    function toggle() {
      isOpen.value = !isOpen.value
      props.$emit('update:open', isOpen.value)
      props.$emit(isOpen.value ? 'open' : 'close')
    }

    const trigger = h('div', {
      staticClass: 'collapse-trigger', on: { click: toggle }
    }, props.$slots.trigger
      ? [props.$slots.trigger({ open: isOpen })]
      : [props.$slots.trigger]
    )

    const content = h('transition', { props: { name: props.animation } }, [
      h('div', {
        staticClass: 'collapse-content',
        attrs: { 'id': props.ariaId, 'aria-expanded': isOpen },
        directives: [{
          name: 'show',
          value: isOpen
        }]
      }, props.$slots.default)
    ])
    return h('div', { staticClass: 'collapse' },
      props.position === 'is-top' ? [trigger, content] : [content, trigger])

  }
}

export default Collapse;
</script>
