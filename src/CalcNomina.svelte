<script>
  import { onMount } from "svelte";

  // TODO

  const minMonthlySalary = 7000000.0;

  let workers = JSON.parse(localStorage.getItem("workers")) ?? [];

  let worker = {
    dni: "",
    fullName: "",
    entryDate: "",
    salaryBaseMonthly: minMonthlySalary,
    utilityDays: 60,
    vacationsDays: 15,
    salaryIntegralMonthly: null,
  };

  $: localStorage.setItem("workers", JSON.stringify(workers));
  $: aporteINCES = totalMonthlyBaseSalary * 0.02;

  let totalMonthlyBaseSalary = 0;
  let totalMonthlyIntegralSalary = 0;
  let totalSSO = 0;
  let totalLPH = 0;
  let aporteINCES = 0;

  let editMode = false;

  let ivss = {
    mrt_weekly:
      (((totalMonthlyBaseSalary * 12) / 52) * 0.04).toFixed("2") ?? null,
    mrt_monthly:
      (
        ((totalMonthlyBaseSalary * 12) / 52) *
        0.04 *
        get_cant_lunes_mes()
      ).toFixed("2") ?? null,
    aporte_patronal: (totalMonthlyBaseSalary * 0.11).toFixed("2") ?? null,
  };

  let faov = {
    workers: totalMonthlyIntegralSalary * 0.01,
    employer: totalMonthlyIntegralSalary * 0.02,
    total: totalMonthlyIntegralSalary * 0.03,
  };

  function aou_worker() {
    if (editMode) {
      update_worker();
    } else {
      add_worker();
    }
    reset_worker();
    update_totals();
    update_ivss_faov();
  }

  function reset_worker() {
    worker = {
      dni: "",
      fullName: "",
      entryDate: "",
      salaryBaseMonthly: minMonthlySalary,
      utilityDays: 60,
      vacationsDays: 15,
      salaryIntegralMonthly: null,
    };
  }
  function del_worker(dni) {
    workers = workers.filter((worker) => worker.dni !== dni);
    update_totals();
  }

  function del_all_workers() {
    workers = [];
    reset_totals();
  }

  function edit_worker(workerEdit) {
    worker = workerEdit;
    editMode = true;
  }

  function add_worker() {
    worker.salaryIntegralMonthly =
      get_integral_daily_salary(
        worker.salaryBaseMonthly,
        worker.utilityDays,
        worker.vacationsDays
      ) * 30;
    workers = [...workers, worker];
  }

  function update_worker() {
    worker.salaryIntegralMonthly =
      get_integral_daily_salary(
        worker.salaryBaseMonthly,
        worker.utilityDays,
        worker.vacationsDays
      ) * 30;
    let updateWorker = {
      ...worker,
    };

    let index = workers.findIndex((wrkr) => {
      wrkr.dni === worker.id;
    });
    workers[index] = updateWorker;
    reset_worker();
    editMode = false;
  }

  function reset_totals() {
    totalMonthlyBaseSalary = 0;
    totalMonthlyIntegralSalary = 0;
    totalSSO = 0;
    totalLPH = 0;
  }

  function update_totals() {
    reset_totals();
    workers.forEach((t) => {
      totalMonthlyBaseSalary += parseFloat(t.salaryBaseMonthly);
      totalMonthlyIntegralSalary += parseFloat(t.salaryIntegralMonthly);
      totalSSO += get_retencion_sso(parseFloat(t.salaryBaseMonthly));
      totalLPH += get_retencion_lph(parseFloat(t.salaryIntegralMonthly));
    });
  }

  function get_retencion_sso(salary_monthly) {
    return ((salary_monthly * 12) / 52) * 0.04;
  }
  function get_retencion_lph(salary_integral_monthly) {
    return salary_integral_monthly * 0.01;
  }
  function get_daily_utilities(salary_monthly, utility_days) {
    return (utility_days / 12 / 30) * (salary_monthly / 30);
  }
  function get_daily_vacations(salary_monthly, vacation_days) {
    return (vacation_days / 12 / 30) * (salary_monthly / 30);
  }

  function get_integral_daily_salary(
    salary_monthly,
    utility_days,
    vacation_days
  ) {
    let comprehensive_salary =
      salary_monthly / 30 +
      get_daily_utilities(salary_monthly, utility_days) +
      get_daily_vacations(salary_monthly, vacation_days);
    return comprehensive_salary;
  }
  function get_cant_lunes_mes() {
    return 4;
  }

  function update_ivss_faov() {
    ivss = {
      mrt_weekly: (((totalMonthlyBaseSalary * 12) / 52) * 0.04).toFixed("2"),
      mrt_monthly: (
        ((totalMonthlyBaseSalary * 12) / 52) *
        0.04 *
        get_cant_lunes_mes()
      ).toFixed("2"),
      aporte_patronal: (totalMonthlyBaseSalary * 0.11).toFixed("2"),
    };
    faov = {
      workers: totalMonthlyIntegralSalary * 0.01,
      employer: totalMonthlyIntegralSalary * 0.02,
      total: totalMonthlyIntegralSalary * 0.03,
    };
  }

  onMount(() => {
    update_totals();
    update_ivss_faov();
  });
</script>

<section>
  <div class="notification is-warning">
    <button class="delete" />
    <strong>En desarrollo</strong>
  </div>
  <header>
    <h1 class="title">Nomina Venezuela</h1>
    <h2 class="subtitle">(IVSS, FAOV, Inces)</h2>
  </header>

  <div class="columns">
    <section class="column">
      <article class=" section content">
        <p>
          Salario minimo mensual vigente:
          <span class="num">{minMonthlySalary.toFixed("2")}</span>
        </p>
        <p>
          Tope 5 Sal. Minimos Calculo de SSO:
          <span class="num">{(minMonthlySalary * 5).toFixed("2")}</span>
        </p>
        <p>
          Tope 10 Sal. Minimos Calculo de PF:
          <span class="num">{(minMonthlySalary * 10).toFixed("2")}</span>
        </p>
      </article>
      <article class="content">
        <h1>notes</h1>
        <p>TODO:</p>
        <ul>
          <li>Calcular dias de vacaciones en base a la fecha de ingreso</li>
        </ul>
        <p>Calculo de IVSS, FAOV e INCES completo</p>
      </article>
    </section>
    <section class="column">
      <article class="content">
        <h1>Añadir o actualizar worker</h1>
        <form on:submit|preventDefault={aou_worker}>
          <label for="cedula">Cedula de identidad (DNI)</label>
          <input
            class="input"
            type="number"
            bind:value={worker.dni}
            placeholder="987123654"
          />
          <label for="nombres y apellidos">Nombre(s) y apellido(s)</label>
          <input
            class="input"
            type="text"
            bind:value={worker.fullName}
            placeholder="Nombre completo"
          />

          <label for="Fecha de ingreso">Fecha de Ingreso</label>
          <input class="input" type="date" bind:value={worker.entryDate} />
          <label for="Sueldo base">Sueldo base mensual </label>
          <input
            class="input num"
            type="text"
            bind:value={worker.salaryBaseMonthly}
            on:blur={() => {
              if (worker.salaryBaseMonthly == "") {
                worker.salaryBaseMonthly = minMonthlySalary;
              }
            }}
            on:click={() => {
              worker.salaryBaseMonthly = "";
            }}
            placeholder="0.00"
          />
          <label for="utilities">Utilidades (30 por mes)</label>
          <div class="field has-addons">
            <p class="control">
              <label for="utility_days" class="label">
                <input
                  class="input is-primary"
                  type="number"
                  bind:value={worker.utilityDays}
                  min="30"
                  step="30"
                />
              </label>
            </p>
            <p class="button is-static">days</p>
          </div>
          <label for="vacations">Vacaciones</label>
          <div class="field has-addons">
            <p class="control">
              <label for="bonus_vacational" class="label">
                <input
                  type="number"
                  class="input is-primary"
                  bind:value={worker.vacationsDays}
                  min="15"
                />
              </label>
            </p>
            <p class="button is-static">days</p>
          </div>
          <button class="button is-primary">
            {#if editMode}
              Update
            {:else}
              Add
            {/if}
          </button>
        </form>
      </article>
    </section>
  </div>

  <section>
    <div class="notification is-warning">
      <button class="delete" />
      <strong>En desarrollo</strong>
    </div>
    <header>
      <h1 class="title">workers list</h1>
    </header>
    <div class="table-container">
      <table class="table is-hoverable">
        <thead>
          <tr>
            <th>#CI</th>
            <th>Nombre(s) y Apellido(s)</th>
            <th>Fecha de ingreso</th>
            <th>Sueldo base Mensual</th>
            <th>V</th>
            <th>U</th>
            <th>Sueldo integral mensual</th>
            <th>Retencion SSO (IVSS)</th>
            <th>Retencion LPH (FAOV)</th>
            <th>Editar</th>
            <th>Borrar</th>
          </tr>
        </thead>
        <tbody>
          {#each workers as worker}
            <tr>
              <td class="num">{worker.dni}</td>
              <td>{worker.fullName}</td>
              <td>{worker.entryDate}</td>
              <td class="num">{worker.salaryBaseMonthly}</td>
              <td>{worker.vacationsDays}</td>
              <td>{worker.utilityDays}</td>
              <td class="num">{worker.salaryIntegralMonthly.toFixed("2")}</td>
              <td class="num">
                {get_retencion_sso(worker.salaryBaseMonthly).toFixed("2")}
              </td>
              <td class="num"
                >{get_retencion_lph(worker.salaryIntegralMonthly).toFixed(
                  "2"
                )}</td
              >
              <td>
                <button class="button is-success" on:click={edit_worker(worker)}
                  >Edit
                </button>
              </td>
              <td>
                <button
                  class="button is-danger"
                  on:click={del_worker(worker.dni)}
                  >Del
                </button>
              </td>
            </tr>
          {/each}
          <tr>
            <td>{workers.length}</td>
            <td colspan="2">Totales</td>
            <td class="num">{totalMonthlyBaseSalary.toFixed("2")} </td>
            <td />
            <td />
            <td class="num">{totalMonthlyIntegralSalary.toFixed("2")}</td>
            <td class="num">{totalSSO.toFixed("2")}</td>
            <td class="num">{totalLPH.toFixed("2")}</td>
            <td />
            <td>
              <button class="button is-danger" on:click={del_all_workers}>
                all
              </button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>

    <div class="notification is-warning">
      <button class="delete" />
      <strong>En desarrollo</strong>
    </div>
    <div class="columns is-multiline">
      <section class="column">
        <article class="content">
          <h1>IVSS</h1>
          <p><cite>Instituto Venezolano del Seguro Social (SSO)</cite></p>
          <p>
            MRT semanal 4%:
            <span class="num">{ivss.mrt_weekly}</span>
          </p>

          <p>
            MRT* Mensual:
            <span class="num">{ivss.mrt_monthly}</span>
          </p>
          <p>
            MRT* Quincenal:
            <span class="num">{ivss.mrt_monthly / 2}</span>
          </p>

          <p>Aporte patronal: Minimo 11% sobre el sueldo del(os) worker(es)</p>
          <p>11%: <span class="num">{ivss.aporte_patronal}</span></p>

          <p>
            Total aporte del mes:
            <span class="box num">
              Total:
              {parseFloat(ivss.mrt_monthly) + parseFloat(ivss.aporte_patronal)}
            </span>
          </p>
          <p><small>*Monto a retener al(los) worker(es) (MRT)</small></p>
        </article>
      </section>
      <section class="column">
        <article class="content">
          <h1>Banavih FAOV</h1>
          <p><cite>Ley de politica habitacional (LPH)</cite></p>

          <p>
            Sueldo integral mensual x 3%:
            <span class="box num">
              Total: {faov.total.toFixed("2")}
            </span>
          </p>
          <p>(1% worker, 2% patrono)</p>
          <p>
            Contributor worker 1%: <span class="num"
              >{faov.workers.toFixed("2")}</span
            >
          </p>
          <p>
            Contributor employer 2%: <span class="num"
              >{faov.employer.toFixed("2")}</span
            >
          </p>
        </article>
      </section>
      <section class="column">
        <article class="content">
          <h1>Inces</h1>
          <p>2% del sueldo total de los workers.</p>
          <p>Pago trimestral</p>
          <p>Aporte al INCES (un mes):</p>
          <p><span class="box num">Total: {aporteINCES.toFixed("2")}</span></p>
        </article>
      </section>
    </div>
  </section>
  <div class="notification is-warning">
    <button class="delete" />
    <strong>En desarrollo</strong>
  </div>
</section>

<style>
  .content {
    color: darkviolet;
  }

  .content h1 {
    text-align: center;
    color: #00d1b2;
  }
  .content p {
    text-align: justify;
  }

  form {
    max-width: 400px;
  }
  .input {
    text-align: right;
  }

  .num {
    font-family: "Linux Biolinum G" !important;
    font-weight: bold;
    font-size: 1.2rem;
  }

  .box.num {
    text-align: center;
  }
</style>
