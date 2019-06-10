<template>
	<div id="app">

		<h1>Calcul de prix</h1>

		<div class="form-group">
			<label for="prix">Prix d'achat</label>
			<input id="prix" type="number" v-model="prices.achat" class="form-control" @change="calcul">
		</div>
		<div class="form-group">
			<label for="marge">Marge</label>
			<input id="marge" type="number" v-model="prices.marge" class="form-control" @change="calcul">
		</div>
		<div class="form-group">
			<label for="fournisseurs">Fournisseurs</label>
			<select name="fournisseurs" ref="fournisseurs" id="fournisseurs" @change="calcul">
				<option v-for="item in fournisseurs " :value="item.remise">{{item.name}}</option>
			</select>
		</div>
		<div class="form-group">
			<label for="tva">TVA</label>
			<input id="tva" type="number" v-model="prices.tva" class="form-control" @change="calcul">
		</div>

		<div class="form-group">
			<label for="achatHT">Total Achat HT</label>
			<input id="achatHT" type="text" v-model="totals.achats + ' €'" readonly>
		</div>
		<div class="form-group">
			<label for="resultHT">Total HT</label>
			<input id="resultHT" type="text" v-model="totals.ht + ' €'" readonly>
		</div>
		<div class="form-group">
			<label for="resultTTC">Total TTC</label>
			<input id="resultTTC" type="text" v-model="totals.ttc + ' €'" readonly>
		</div>


	</div>
</template>

<script>

    export default {
        name: 'App',
        data: function () {
            return {
                prices: {
                    achat: 100,
                    marge: 25,
                    tva: 20,
                },
                totals: {
                    ht: 0,
                    ttc: 0,
                    achats: 0,
                },
                fournisseurs: [
                    {
                        name: 'autre',
                        remise: 0,
                    },
                    {
                        name: 'continental',
                        remise: 45.88
                    },
                    {
                        name: 'uniroyal',
                        remise: 42.5
                    }
                ]
            }
        },
        methods: {
            calcul: function () {
                let priceAchat;
                if(parseFloat(this.$refs['fournisseurs'].value) > 0){
                    priceAchat =    parseFloat(this.prices.achat) * ((100 - this.$refs['fournisseurs'].value)/100);
                } else {
                    priceAchat =    parseFloat(this.prices.achat);
                }
                this.totals.achats = Math.round(priceAchat * 100) / 100;
                this.totals.ht =    Math.round(priceAchat / ((100 - parseFloat(this.prices.marge)) / 100) * 100) / 100;
                let tva =           (priceAchat * (parseFloat(this.prices.tva) + 100) / 100);
                this.totals.ttc =   Math.round(tva / ((100 - parseFloat(this.prices.marge)) / 100) * 100) / 100;
            }
        }
    }
</script>

<style>
	#app {
		font-family: 'Avenir', Helvetica, Arial, sans-serif;
		-webkit-font-smoothing: antialiased;
		-moz-osx-font-smoothing: grayscale;
		color: #2c3e50;
		margin-top: 60px;
	}

	.form-group {
		display: block;
		margin: 0.5rem 0;
	}

	label {
		text-align: right;
		padding-right: 10px;
		min-width: 130px;
		display: inline-block;
		text-transform: uppercase;
		font-size: 12px;
		letter-spacing: 0.05em;
	}

	select,
	input {
		display: inline-block;
		max-width: 250px;
		width: 100%;
		padding: 0.25rem;
		font-size: 16px;
		border: 1px solid #e0e0e0;
	}
</style>
