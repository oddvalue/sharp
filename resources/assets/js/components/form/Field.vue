<script>
    import Fields, { NameAssociation as fieldCompName } from './fields/index';
    import util from '../../util';

    export default {
        name:'SharpField',
        components: Fields,

        provide() {
            return {
                $field: this
            }
        },

        props: {
            fieldKey: String,
            fieldType:  String,
            fieldProps: Object,
            fieldLayout: Object,
            value: [String, Number, Boolean, Object, Array],
            updateData: Function
        },
        mounted() {
            //console.log(this);
        },
        render(h) {
            if(!(this.fieldType in fieldCompName)) {
                util.error(`SharpField '${this.fieldKey}', unknown type '${this.fieldType}'`, this.fieldProps);
                return null;
            }

            let { key, ...fieldProps } = this.fieldProps;

            //console.log(fieldCompName[this.fieldType]);

            return h(fieldCompName[this.fieldType],{
                props : {
                    fieldKey:this.fieldKey,
                    fieldLayout:this.fieldLayout,
                    value:this.value,
                    ...fieldProps
                },
                on: {
                    input: val => {
                        this.updateData(this.fieldKey,val);
                    }
                }
            });
        }
    }
</script>