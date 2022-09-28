<template>

    <div class="card card-compact w-96 shadow-xl p-7 bg-base-300 mt-4">
        <h1 class="card-title justify-center">AM-03127-LED-API Controller</h1>
        <div class="card-body">
            <form class="form-control" v-on:submit.prevent="submitData">

                <label class="label">
                    <span class="label-text">Input String</span>
                </label>
                <input class="input input-bordered"  v-model="inputString">

                <label class="label">
                    <span class="label-text">Lead Effect</span>
                </label>
                <select class="select select-bordered" v-model="leadEffect">
                    <option selected value="<FA>">Immediate</option>
                    <option value="<FB>">Xopen</option>
                    <option value="<FC>">Curtain Up</option>
                    <option value="<FD>">Curtain Down</option>
                    <option value="<FE>">Scroll Left</option>
                    <option value="<FF>">Scroll Right</option>
                </select>

                <label class="label">
                    <span class="label-text">Speed</span>
                </label>
                <select class="select select-bordered" v-model="speed">
                    <option selected value="<MA>">Fast</option>
                    <option value="<MB>">Fast Blink</option>
                    <option value="<MC>">Fast Song 1</option>
                    <option value="<MD>">Fast Song 2</option>
                    <option value="<ME>">Fast Song 3</option>
                    <option value="<MQ>">Medium Fast</option>
                    <option value="<MR>">Medium Fast Blink</option>
                    <option value="<MS>">Medium Fast Song 1</option>
                    <option value="<MT>">Medium Fast Song 2</option>
                    <option value="<MU>">Medium Fast Song 3</option>
                    <option value="<Ma>">Medium Slow</option>
                </select>

                <label class="label">
                    <span class="label-text">Lag Effect</span>
                </label>
                <select class="select select-bordered" v-model="lagEffect">
                    <option value="<FA>">Immediate</option>
                    <option value="<FB>">Xopen</option>
                    <option value="<FC>">Curtain Up</option>
                    <option value="<FD>">Curtain Down</option>
                    <option value="<FE>">Scroll Left (Recommended)</option>
                    <option value="<FG>">Scroll Right</option>
                </select>                

                <button type="submit" class="btn btn-primary mt-7">Submit Data</button>
                
            </form>
            <button class="btn btn-error mt-7" @click="resetToFactoryDefaults">Reset to Factory Defaults</button>
        </div>
    </div>        
    
</template>

<script>
import axios from 'axios';

export default {
    data() {
        return {
            form: new FormData,
            inputString: "",
            leadEffect: "",
            speed: "",
            lagEffect: ""
        }
    },
    methods: {
        submitData: async function () {
            this.form.delete('data');
            this.form.append('data', `<L1><PA>${this.leadEffect}${this.speed}<WA>${this.lagEffect}<AA><CC><N00>${this.inputString}`)
            axios.post(import.meta.env.VITE_API_URL, this.form).then((response) => console.log(response.data))
            console.log(this.leadEffect)
        },
        resetToFactoryDefaults: async function () {
            this.form.delete('data')
            if (confirm("Are you sure you want to reset the board to factory defaults?")) {
                this.form.append('data', "<D*>")
                axios.post(import.meta.env.VITE_API_URL, this.form).then((response) => console.log(response.data))
            } else return
        }
    }
}
</script>