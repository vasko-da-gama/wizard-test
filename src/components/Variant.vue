<template>

    <div v-bind:style="{borderColor: description.color }" class="variant">

        <div class="var_sides left_side">
            <p>{{description.title}}</p>
            <div class="variant_description">
                <p><span v-for="line in description.description.split('\n')" v-bind:key="line">{{line}}<br/><br/></span></p>
            </div>
        </div>

        <div class="var_sides right_side">
            <p>{{localPrice | format}} ₽</p>
            <div class="options">
                <form action="">

                    <label v-for="opt in description.options" v-bind:key="opt">
                        <div>
                            <input v-on:change="checkboxChanged" v-bind:cur_price="opt.price" type="checkbox" name="opt1" value="0" />
                            {{opt.title}}
                        </div>
                    </label>

                    <span v-if="description.select[0]">{{description.select[0].title}}</span>
                    <select v-if="description.select[0]" v-on:change="selectChanged" name="" id="">
                        <option v-for="item in description.select[0].items" v-bind:key="item" v-bind:value="item.price">{{item.title}}</option>
                    </select>


                    <button v-on:click="confirmThisVariant">Выбрать</button>
                </form>
            </div>
        </div>

    </div>

    
</template>

<script>
export default {
    name: "Variant",
    data() {
        return {
            localPrice: this.description.price_default + (
                this.description.select.length > 0 ? this.description.select[0].items[0].price : 0
            ),
        }
    },
    props: {
        description: Object,
        varId: Number,
        registerCurrentVariant: Function,
    },
    methods: {
        checkboxChanged: function (e) {
            let checkbox = e.target,
                current_price = Number.parseInt(checkbox.attributes.cur_price.value);
            
            this.localPrice += ( // this.description.price_default + 
                checkbox.checked ? current_price : -current_price
            );
        },
        selectChanged: function (e) {
            let select = e.target,
                current_price = Number.parseInt(select.value);

            this.localPrice = this.description.price_default + current_price;
        },
        confirmThisVariant: function (e) {
            e.preventDefault();
            this.registerCurrentVariant(this.localPrice);

        },
    },
    filters: {
      format: val => `${val}`.replace(/(\d)(?=(\d{3})+([^\d]|$))/g, '$1 '),
    },
}
</script>