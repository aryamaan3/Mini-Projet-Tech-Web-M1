<template>
  <div class="menu">
    <h2>Carte du restaurant</h2>
    <div class="panier" align="right">
      <md-badge class="md-primary" :md-content="lenPanier">
        <md-button class="md-icon-button" v-on:click="panierClicked()">
          <md-icon>shopping_bag</md-icon>
        </md-button>
      </md-badge>
    </div>
    <md-dialog :md-active.sync="showDialog">
      <md-dialog-title>Votre Commande</md-dialog-title>

      <md-list>
        <md-list-item v-for="index in lenPanier" :key="index">
          <md-list-item-text>
            <md-list-item-title>{{
              panier.plat[index - 1]
            }}</md-list-item-title>
            <md-list-item-subtitle>
              {{ panier.prix[index - 1] }} €</md-list-item-subtitle
            >
          </md-list-item-text>
          <md-list-item-action>
            <md-button
              class="md-icon-button"
              v-on:click="removeFromPanier(index - 1)"
            >
              <md-icon>remove_circle</md-icon>
            </md-button>
          </md-list-item-action>
        </md-list-item>
        <md-list-item>
          <md-list-item-text>
            <md-list-item-title>Total</md-list-item-title>
            <md-list-item-subtitle> {{ total }} €</md-list-item-subtitle>
          </md-list-item-text>
        </md-list-item>
      </md-list>

      <form novalidate class="md-layout" @submit.prevent="validateUser">
        <md-card-content>
          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="last-name">Nom</label>
                <md-input
                  name="last-name"
                  id="last-name"
                  autocomplete="family-name"
                  v-model="form.nom"
                />
              </md-field>
            </div>
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="first-name">Prenom</label>
                <md-input autocomplete="given-name" v-model="form.prenom" />
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="adresse">Addresse</label>
                <md-input v-model="form.adresse"></md-input>
                <span class="md-error">L'addresse est nécessaire</span>
              </md-field>
            </div>
          </div>

          <div class="md-layout md-gutter">
            <div class="md-layout-item md-small-size-100">
              <md-field>
                <label for="paiement">Paiement</label>
                <md-select v-model="form.paiement">
                  <md-option></md-option>
                  <md-option value="M">Espèces</md-option>
                  <md-option value="F">CB</md-option>
                </md-select>
              </md-field>
            </div>
          </div>
        </md-card-content>
      </form>

      <md-dialog-actions>
        <md-button class="md-accent" @click="showDialog = false"
          >Annuler</md-button
        >
        <md-button class="md-primary" @click="commanderClicked"
          >Commander</md-button
        >
      </md-dialog-actions>
      <md-dialog-alert
        :md-active.sync="formError"
        md-content="Veuillez remplir tous les champs"
        md-confirm-text="OK"
      />
    </md-dialog>
    <md-dialog-alert
      :md-active.sync="formSent"
      md-content="Merci pour votre commande!"
      md-confirm-text="X"
    />

    <h3>Menus</h3>
    <md-list class="md-triple-line">
      <md-list-item>
        <md-avatar>
          <img
            :src="`https://fac.img.pmdstatic.net/fit/http.3A.2F.2Fprd2-bone-image.2Es3-website-eu-west-1.2Eamazonaws.2Ecom.2FFAC.2Fvar.2Ffemmeactuelle.2Fstorage.2Fimages.2Fminceur.2Fnews-minceur.2Fheure-ideale-repas-science-imc-41576.2F14629249-1-fre-FR.2Fquelle-est-l-heure-ideale-pour-prendre-ses-repas-d-apres-la-science.2Ejpg/750x562/quality/80/crop-from/center/cr/wqkgVGhpbmtzdG9jayAvIEZlbW1lIEFjdHVlbGxl/quelle-est-l-heure-ideale-pour-prendre-ses-repas-d-apres-la-science.jpeg`"
            alt="People"
          />
        </md-avatar>
        <div class="md-list-item-text">
          <span>Formule Dejeuner - 25€</span>
          <span>Specialité Niçoise</span>
          <p>Plat du jour, verre de vin et café gourmand</p>
        </div>
        <md-button v-on:click="addToPanier('Formule Dejeuner', '25')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://www.sortiraparis.com/images/80/83679/432663-cafe-laitcha-a-paris-les-photos-28.jpg"
            alt="People"
          />
        </md-avatar>
        <div class="md-list-item-text">
          <span>Menu Musée - 38€</span>
          <span>Servi le midi et le soir</span>
          <p>Entrée, Plat, Fromage et dessert</p>
        </div>
        <md-button v-on:click="addToPanier('Menu Musée', '38')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://chateauderochegude.diadabox.fr/2735/menu-degustation.jpg"
            alt="People"
          />
        </md-avatar>
        <div class="md-list-item-text">
          <span>Menu Degustation - 60€</span>
          <span>Servi le midi et le soir</span>
          <p>
            La Blette, L'Artichaut violet bouquet, Le Tortellini, La Caille,
            Fromage, La Fôret Noire
          </p>
        </div>
        <md-button v-on:click="addToPanier('Menu Degustation', '60')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
    </md-list>
    <h3>À la carte</h3>
    <md-list>
      <h4>Entrée</h4>
      <md-list-item>
        <md-avatar>
          <img
            src="https://cdn-elle.ladmedia.fr/var/plain_site/storage/images/elle-a-table/recettes-de-cuisine/petits-artichauts-violets-a-l-italienne-2077962/22038582-3-fre-FR/Petits-artichauts-violets-a-l-italienne.jpg"
            alt="People"
          />
        </md-avatar>

        <span class="md-list-item-text">L'Artichaut Violet Bouquet - 22€</span>
        <md-button
          v-on:click="addToPanier('L\'Artichaut Violet Bouquet', '22')"
        >
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://www.francine.com/media/cache/recipe_big/uploads/media/recipe/0001/05/terrine-foie-gras-lutee-jpg_6463bd977f6b3cfdc461960200fdd758a08c78a4.jpeg"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">Le Foie Gras - 24€</span>
        <md-button v-on:click="addToPanier('Le Foie Gras', '24')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://d1e3z2jco40k3v.cloudfront.net/-/media/ducrosfr-2016/recipes/2000/coquilles_saint_jacques_minute_2000.jpg?rev=ed34abec2b244e4fa8fa1ea3edb7ab09&vd=20200704T131406Z&hash=A75442E94B4A0A9DB332796CA2A6F07A"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">La Saint Jacques - 25€</span>
        <md-button v-on:click="addToPanier('La Saint Jacques', '25')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <h4>Plat</h4>
      <md-list-item>
        <md-avatar>
          <img
            src="https://www.nahrin.ch/cms/fileadmin/Bilder/Rezepte/Risotto_Grundrezept_Pilze.jpg"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">Le Risotto - 30€</span>
        <md-button v-on:click="addToPanier('Le Risotto', '30')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://assets.bonappetit.com/photos/604104ee8fd036af451e9f0a/5:7/w_1405,h_1967,c_limit/Basically-Tortellini-Soup.jpg"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">Le Tortellini - 32€</span>
        <md-button v-on:click="addToPanier('Le Tortellini', '32')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://t2.uc.ltmcdn.com/fr/images/5/0/6/recette_de_gambas_au_four_avec_du_vin_blanc_14605_600_square.jpg"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">Le Gambas - 32€</span>
        <md-button v-on:click="addToPanier('Le Gambas', '32')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://fac.img.pmdstatic.net/fit/http.3A.2F.2Fprd2-bone-image.2Es3-website-eu-west-1.2Eamazonaws.2Ecom.2Ffac.2F2018.2F07.2F30.2F89f873ef-5d90-4c1e-8e01-6189e2988ddd.2Ejpeg/750x562/quality/80/crop-from/center/cr/wqkgSmFuIEhlbmRyaWsgVmFuIERlciBXZXN0aHVpemVuLCAvIEZlbW1lIEFjdHVlbGxl/entrecotes-salade-verte-et-pommes-de-terre-grillees.jpeg"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">Le Boeuf - 38€</span>
        <md-button v-on:click="addToPanier('Le Boeuf', '38')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <h4>Dessert</h4>
      <md-list-item>
        <md-avatar>
          <img
            src="https://res.cloudinary.com/hv9ssmzrz/image/fetch/c_fill,f_auto,h_600,q_auto,w_800/https://s3-eu-west-1.amazonaws.com/images-ca-1-0-1-eu/tag_photos/original/27147/café-gourmand_flickr_5497167092_5629877990_b.jpg"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">Le Café Gourmand - 10€</span>
        <md-button v-on:click="addToPanier('Le Café Gourmand', '10')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
      <md-list-item>
        <md-avatar>
          <img
            src="https://assets.afcdn.com/recipe/20210511/120269_w1024h1024c1cx1060cy707.jpg"
            alt="People"
          />
        </md-avatar>
        <span class="md-list-item-text">La Fôret Noire - 12€</span>
        <md-button v-on:click="addToPanier('La Fôret Noire', '12')">
          <md-icon>shopping_cart</md-icon>
        </md-button>
      </md-list-item>
    </md-list>
  </div>
</template>

<script>
export default {
  name: "Menu",
  props: ["menu"],
  data: () => ({
    form: {
      prenom: null,
      nom: null,
      adresse: null,
      paiement: null,
    },
    panier: {
      plat: [],
      prix: [],
    },
    total: 0,
    lenPanier: 0,
    showDialog: false,
    formError: false,
    formSent: false,
  }),
  mounted() {},
  methods: {
    panierClicked() {
      this.showDialog = true;
    },
    addToPanier(nomPlat, prixPlat) {
      this.panier.plat.push(nomPlat);
      this.panier.prix.push(parseInt(prixPlat));
      this.total += parseInt(prixPlat);
      this.lenPanier += 1;
    },
    isFormValid() {
      return (
        this.form.prenom &&
        this.form.nom &&
        this.form.adresse &&
        this.form.paiement &&
        this.total
      );
    },
    removeFromPanier(index) {
      this.total -= this.panier.prix[index];
      this.panier.plat.splice(index, 1);
      this.panier.prix.splice(index, 1);
      this.lenPanier -= 1;
    },
    commanderClicked() {
      if (this.isFormValid()) {
        this.showDialog = false;
        this.formSent = true;
        // reload page after 5 seconds
        setTimeout(() => {
          window.location.reload();
        }, 2000);
      } else {
        this.formError = true;
      }
    },
  },
};
</script>

<style scoped>
</style>
