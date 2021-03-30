<template>
    <div id="app">
        <div class="fraction">
            <template v-for="(num, i) in nums">
                <div class="fraction__summand">
                    <div v-if="nums.length > 2" @click="deleteSummand(i)" class="fraction__delete">&times;</div>
                    <input @input="checkNumber($event, i, 'numerator')" v-model="num.numerator" type="text">
                    <hr>
                    <input @input="checkNumber($event, i, 'denominator')" v-model="num.denominator" type="text">
                </div>
                <div v-if="i < nums.length - 1" class="plus">+</div>
            </template>
            <div class="equal">=</div>
            <div class="result">{{result}}</div>
        </div>
        <button :disabled="nums.length >= 5" @click="addSummand">add summand</button>
    </div>
</template>
<script>
export default {
    name: 'App',
    data() {
        return {
            nums: [{
                    numerator: "1",
                    denominator: "5"
                },
                {
                    numerator: "2",
                    denominator: "3"
                },
            ],
            max: 99,
            min: 1

        }
    },
    computed: {
        result() {
            let validItems = this.validSummands
            if (!validItems.length) return
            return validItems.map(num => num.numerator / num.denominator).reduce((accumulator, currentValue) => accumulator + currentValue).toPrecision(4)
        },
        validSummands() {
            return this.nums.filter(num => num.numerator !== "" && num.denominator !== "")
        }
    },
    methods: {
        addSummand() {
            if (this.nums.length >= 5) return
            this.nums.push({
                numerator: "",
                denominator: ""
            })
        },
        deleteSummand(summand) {
            this.nums = this.nums.filter((num, idx) => idx !== summand)
        },
        checkNumber(e, i, place) {
            this.nums[i][place] = e.target.value.replace(/[^0-9]/g, '')
            if (parseInt(e.target.value) > this.max) {
                this.nums[i][place] = this.max
            } else if (parseInt(e.target.value) < this.min) {
                this.nums[i][place] = this.min
            }
        }
    }
}
</script>
<style lang="css">
.fraction {
    display: flex;
    align-items: center;
    margin: 0 0 30px 0;
}

.fraction__summand {
    min-width: 60px;
    position: relative;
}

.fraction__delete {
    position: absolute;
    right: 0;
    top: 0;
    cursor: pointer;

}

.fraction__summand input {
    max-width: 40px;
}

.plus {
    margin: 0 15px;
}

.equal {
    margin: 0 15px;
}
</style>