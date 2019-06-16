<script>
  import * as _ from "lodash";

  export default {
    methods: {
      notifyUpdate(eventName, origin, modifyLogic) {
        let copy = _.cloneDeep(origin);
        modifyLogic(copy);

        this.$emit(eventName || 'input', copy);
      },
    }
  }
</script>
