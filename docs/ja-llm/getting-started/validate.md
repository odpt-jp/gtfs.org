# GTFSフィードの品質を評価する {: #evaluate-your-gtfs-feeds-quality}


高品質なGTFSは、完全で、正確で、最新のものです。これは、サービスが現在どのように運行されているかを表し、可能な限り多くの情報を提供することを意味します。

## 完全なデータ {: #complete-data}


高品質な GTFS には、祝日および夏季のダイヤ変更、正確な停留所等(stop)の位置、ならびに他の乗客向け資料と一致するルート・路線系統(route)および行先表示(headsign)の名称など、重要なサービス詳細が含まれます。事業者が GTFS の作成をベンダーに委託している場合でも、印刷物、車内、およびオンラインで提示される情報に一貫性があることを確保する最終的な責任は事業者にあります。

## 正確なデータ {: #accurate-data}


正確なデータは、公共交通機関の乗客に信頼性が高く使いやすい移動体験を提供するために不可欠です。データ内のエラーにより、データセットの一部または全体が利用できなくなる可能性があります。

## 最新のデータ {: #up-to-date-data}


古いデータは、利用可能なフィードがないことよりも問題になる場合があります。単に情報を公開するだけでは十分ではなく、乗客が目にし、体験する内容と一致していなければなりません。最大規模の交通事業者の一部は GTFS を毎週、あるいは毎日更新していますが、ほとんどの事業者は数か月ごと、または運行変更時に年に数回、GTFS を更新する必要があります。これには、新しいルート・路線系統(route)や停留所等(stop)、時刻表の変更、運賃体系の更新などが含まれます。

多くの事業者は、GTFS の作成および管理をベンダーに委託しています。一部のベンダーは運行変更について積極的に問い合わせる場合がありますが、事業者が今後の運行変更についてベンダーと連絡を取ることが重要です。運行変更を含む GTFS を事前に公開することで、事業者、ベンダー、経路検索サービス、乗客のすべてにとって、移行が円滑に進むようにすることができます。

## 公式バリデーターの使用 {: #using-official-validators}


公式GTFSバリデーターは、公式仕様に照らしてデータセットの品質を評価し、高品質なデータセットを構成するものについて業界内で共通の理解を確保します。 

[MobilityData](https://mobilitydata.org/)が保守する無料かつオープンソースの[Canonical GTFS Schedule validator](https://gtfs-validator.mobilitydata.org/)[^1]は、GTFSデータが公式の[GTFS Schedule Reference](../../documentation/schedule/reference/)および[Best Practices](../../documentation/schedule/schedule_best_practices)に準拠していることを確認します。ほかの関係者と共有できる使いやすいレポートと、包括的なドキュメントを提供します。

<div class="usage">
    <div class="usage-list">
        <ol>
            <li><a href="https://gtfs-validator.mobilitydata.org/">gtfs-validator.mobilitydata.org</a>にアクセスします。 </li>
            <li>GTFSデータセットを読み込みます。ZIPファイルを選択またはドラッグ＆ドロップするか、URLをコピー＆ペーストできます。</li>
            <li>検証が完了すると、レポートを開くためのオプションが表示されます。</li>
            <li>バリデーターがデータ内の問題を検出したかどうか、およびそれらを修正する方法に関するドキュメントへのリンクを確認できます。検証レポートのURLは30日間有効であり、ほかの人と共有できます。</li>
        </ol>
    </div>
    <div class="usage-video">
        <video class="center" width="560" height="315" controls>
            <source src="../../assets/validator-demo-large.mp4" type="video/mp4">
        </video>
    </div>
</div>

同様に、無料かつオープンソースの標準的な[GTFS Realtime Validator](https://github.com/MobilityData/gtfs-realtime-validator)を使用することで、GTFSデータが公式の[GTFS Realtime Reference](../../documentation/realtime/reference/)および[Best Practices](../../documentation/realtime/realtime_best_practices)に準拠していることを確認できます。

高品質なデータの作成に関する情報については、[California Transit Data Guidelines](https://dot.ca.gov/cal-itp/california-transit-data-guidelines)、[GTFS Schedule Best Practices](../../documentation/schedule/schedule_best_practices)、および[GTFS Realtime Best Practices](../../documentation/realtime/realtime_best_practices)を参照してください。

[^1]: データパイプラインでこのツールを使用する方法に関する詳細な手順、およびこのプロジェクトへの貢献については、[GitHub repository](https://github.com/MobilityData/gtfs-validator)を参照してください。
