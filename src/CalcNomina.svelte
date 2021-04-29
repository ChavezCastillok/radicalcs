<script>
  // TODO
</script>

<section>
  <header>
    <h1 class="title">IVSS, FAOV, Inces</h1>
  </header>
  <main class="columns section content is-multiline">
    <section class="column is-half-tablet is-4-desktop">
      <h1>Salario base</h1>
      <article class="box mt-3">
        <article class="field has-addons">
          <p class="control has-icons-left">
            <input
              class="input is-primary"
              type="text"
              name="sueldo"
              id="sueldo_month"
              placeholder="400000.00"
            />
            <span class="icon is-left">
              <ion-icon size="small" name="cash-outline" />
            </span>
          </p>
          <p class="button is-static">Mensual</p>
        </article>
      </article>
      <article class="field has-addons">
        <p class="control">
          <input
            class="input is-disabled"
            type="text"
            id="sueldo_year"
            value="0.00"
            readonly
          />
        </p>
        <p class="control">
          <!-- svelte-ignore a11y-missing-attribute -->
          <a class="button is-static">Anual</a>
        </p>
      </article>

      <article class="field has-addons">
        <p class="control">
          <input
            class="input is-disabled"
            type="text"
            id="sueldo_week"
            value="0.00"
            readonly
          />
        </p>
        <p class="button is-static">Semanal</p>
      </article>
      <article class="field has-addons">
        <p class="control">
          <input
            class="input is-disabled"
            type="text"
            id="sueldo_day"
            value="0.00"
            readonly
          />
        </p>
        <p class="button is-static">Diario</p>
      </article>
    </section>
    <section class="column is-half-tablet is-4-desktop">
      <h1>+</h1>
      <article class="box">
        <article class="field has-addons">
          <p class="control has-icons-left">
            <label for="alimentation_bonus" class="label">
              <input
                class="input is-primary"
                type="text"
                id="alimentation_bonus"
                placeholder="0.00"
              />
            </label>
            <span class="icon is-left">
              <ion-icon size="small" name="cash-outline" />
            </span>
          </p>
          <p class="button is-static">Bono</p>
        </article>
      </article>
      <article>
        <p>Utilidades: <span id="utilidades">0.00</span> por dia.</p>
        <div class="field has-addons">
          <p class="control">
            <label for="utility_days" class="label">
              <input
                class="input is-primary"
                type="number"
                id="utility_days"
                value="60"
              />
            </label>
          </p>
          <p class="button is-static">dias</p>
        </div>
      </article>
      <article>
        <p>Vacaciones: <span id="vacaciones">0.00</span> por dia.</p>
        <div class="field has-addons">
          <p class="control">
            <label for="bonus_vacational" class="label">
              <input
                type="number"
                class="input is-primary"
                id="bonus_vacational"
                value="15"
              />
            </label>
          </p>
          <p class="button is-static">dias</p>
        </div>
      </article>
    </section>

    <section class="column is-half-tablet is-4-desktop">
      <h1>Salario Integral</h1>
      <article class="field has-addons">
        <p class="control">
          <input
            class="input is-disabled"
            type="text"
            id="sueldo_integral_mensual"
            value="0.00"
            readonly
          />
        </p>
        <p class="button is-static">Mensual</p>
      </article>
      <article class="field has-addons">
        <p class="control">
          <input
            class="input is-disabled"
            type="text"
            id="sueldo_integral_anual"
            value="0.00"
            readonly
          />
        </p>
        <p class="button is-static">Anual</p>
      </article>
      <article class="field has-addons">
        <p class="control">
          <input
            class="input is-disabled"
            type="text"
            id="sueldo_integral_semanal"
            value="0.00"
            readonly
          />
        </p>
        <p class="button is-static">Semanal</p>
      </article>
      <article class="field has-addons">
        <p class="control">
          <input
            class="input is-disabled"
            type="text"
            id="sueldo_integral_diario"
            value="0.00"
            readonly
          />
        </p>
        <p class="button is-static">Diario</p>
      </article>
    </section>

    <section class="column is-half-tablet is-one-third-desktop">
      <h1>IVSS</h1>
      <p><cite>Instituto Venezolano del Seguro Social (SSO)</cite></p>

      <p />
      <p>semanal 4%: <span id="mrt_week">0.00</span></p>

      <p>MRT* Mensual: <span id="mrt_month">0.00</span></p>
      <p>MRT* Quincenal: <span id="mrt_quincena">0.00</span></p>

      <p>Aporte patronal: Minimo 11% sobre el sueldo del trabajador</p>
      <p>11%: <span id="aporte_patrono_ivss">0.00</span></p>

      <p>Total aporte del mes: <span id="aporte_ivss_mes">0.00</span></p>
      <p><small>*Monto a retener al trabajador (MRT)</small></p>
    </section>

    <section class="column">
      <h1>FAOV</h1>
      <p><cite>Ley de politica habitacional (LPH)</cite></p>

      <p>Sueldo mensual x 3%: <span id="aporte_faov">0.00</span></p>
      <p>(1% trabajador, 2% patrono)</p>
      <p>Aporte trabajador 1%: <span id="mrt_lph">0.00</span></p>
      <p>Aporte patrono 2%: <span id="aporte_lph_patrono">0.00</span></p>
      <p>Total: <span id="total_lph_faov">0.00</span></p>
    </section>

    <section class="column">
      <h1>Inces</h1>
      <p>2% del sueldo trimestral</p>
      <p>Asumiendo que el sueldo es el mismo los tres meses: ↓</p>
      <p>Aporte al INCES: <span id="aporte_inces">0.00</span></p>
    </section>
  </main>
</section>

<style>
  input,
  span {
    font-family: "Linux Biolinum G" !important;
  }

  span {
    font-weight: bold;
    font-size: 1.2rem;
  }

  ion-icon {
    color: #00d1b2;
  }

  .content {
    color: darkviolet;
  }

  .content h1 {
    text-align: center;
    color: #00d1b2;
  }
  .input,
  .content p {
    text-align: right;
  }

  @media (orientation: portrait) {
    main {
      flex-direction: column;
    }
  }
</style>
