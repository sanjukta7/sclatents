<!-- ===== Datasets ===== -->
  <h2>Datasets</h2>
  <div class="table-wrap">
    <table>
      <thead>
        <tr>
          <th>Dataset</th>
          <th>Cells</th>
          <th>Perturbations</th>
          <th>Use</th>
        </tr>
      </thead>
      <tbody>
        <tr><td>Parse 1M (PBMCs)</td><td>~1M</td><td>90 cytokines, 12 donors</td><td>Main benchmark</td></tr>
        <tr><td>Replogle (HepG2 / Jurkat / RPE1)</td><td>—</td><td>372–1000+ CRISPR KOs</td><td>Main benchmark</td></tr>
        <tr><td>Wholebrain CRISPR atlas (Shi et al. 2026)</td><td>6.36M</td><td>~2600 in-vivo</td><td>Case study</td></tr>
        <tr><td>Pfizer IMRU (OOD)</td><td>864k</td><td>1732 CRISPRi targets</td><td>Biology / drug target eval</td></tr>
      </tbody>
    </table>
  </div>

  <!-- ===== Results ===== -->
  <h2 id="results">Results</h2>

  <p><strong>Distribution metrics</strong> vs. scLDM &omega;=1 baseline:</p>
  <div class="table-wrap">
    <table>
      <thead>
        <tr><th>Metric</th><th>evae best</th><th>scLDM</th><th>Improvement</th></tr>
      </thead>
      <tbody>
        <tr><td>Parse W2 &darr;</td><td>11.89 (flow-mse)</td><td>12.46</td><td>~5%</td></tr>
        <tr><td>Parse MMD² &darr;</td><td class="best">0.0041 (AR-ce-quantile)</td><td>0.027</td><td class="best">6.6&times;</td></tr>
        <tr><td>Parse FD &darr;</td><td class="best">1.84 (AR-ce-quantile)</td><td>18.14</td><td class="best">9.9&times;</td></tr>
        <tr><td>Replogle W2 &darr;</td><td>7.87 (AR-ce-quantile)</td><td>11.29</td><td>~30%</td></tr>
        <tr><td>Replogle MMD² &darr;</td><td class="best">0.0087 (AR-ce-quantile)</td><td>0.20</td><td class="best">23&times;</td></tr>
      </tbody>
    </table>
  </div>

  <p><strong>Biology / OOD</strong> — Pfizer NF-&kappa;B reversion task:</p>
  <div class="table-wrap">
    <table>
      <thead>
        <tr><th>Model</th><th>Enrichment AUC</th><th>Selectivity AUC</th></tr>
      </thead>
      <tbody>
        <tr><td>Parse-trained fsq-hurdle VAE</td><td class="best">0.792</td><td>0.750</td></tr>
        <tr><td>Replogle-trained gaus-hurdle VAE</td><td>0.758</td><td>—</td></tr>
        <tr><td>scGPT (reference)</td><td>~0.80</td><td>—</td></tr>
      </tbody>
    </table>
  </div>

  <div class="callout">
    Near-parity with scGPT on biology tasks using <strong>1000&times; less pretraining data</strong>.
    Top-5 reversion targets correctly include <strong>RELA</strong> and <strong>RBCK1</strong> — canonical NF-&kappa;B activators.
  </div>
