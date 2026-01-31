import { useState } from "react";
import { Card, CardContent } from "@/components/ui/card";
import { Button } from "@/components/ui/button";
import { Plus, Factory, Globe, CalendarClock } from "lucide-react";

const stages = [
  "初回接触",
  "要件確認",
  "見積提出",
  "検討中",
  "交渉中",
  "成約",
  "失注",
];

const stageColors = {
  "初回接触": "bg-gray-100",
  "要件確認": "bg-blue-50",
  "見積提出": "bg-indigo-50",
  "検討中": "bg-yellow-50",
  "交渉中": "bg-orange-50",
  "成約": "bg-green-50",
  "失注": "bg-red-50",
};

export default function OverseasSalesDashboard() {
  const [view, setView] = useState("all");
  const [deals] = useState([
    {
      id: "DEAL-001",
      customer: "ABC Sauna Ltd.",
      country: "Germany",
      product: "Outdoor Sauna",
      stage: "見積提出",
      nextAction: "顧客フィードバック確認",
      nextDate: "2026-02-03",
      factoryStatus: "生产中",
    },
    {
      id: "DEAL-002",
      customer: "Nordic Spa Group",
      country: "Finland",
      product: "Barrel Sauna",
      stage: "交渉中",
      nextAction: "价格再协商",
      nextDate: "2026-02-01",
      factoryStatus: "待排产",
    },
  ]);

  const filteredDeals = view === "all" ? deals : deals.filter((d) => d.stage === view);

  return (
    <div className="p-6 space-y-6">
      <h1 className="text-2xl font-semibold">海外営業ダッシュボード</h1>

      {/* 第一排：销售阶段 */}
      <div className="space-y-2">
        <div className="text-sm text-gray-500">① 営業ステージ</div>
        <div className="flex gap-2 flex-wrap">
          <Button size="sm" variant={view === "all" ? "default" : "outline"} onClick={() => setView("all")}>
            <Globe size={14} className="mr-1" /> 全案件
          </Button>
          {stages.map((s) => (
            <Button key={s} size="sm" variant={view === s ? "default" : "outline"} onClick={() => setView(s)}>
              {s}
            </Button>
          ))}
        </div>
      </div>

      {/* 第二排：案件卡片 */}
      <div className="space-y-2">
        <div className="text-sm text-gray-500">② 案件一覧（営業 × 工場）</div>
        <div className="grid grid-cols-1 md:grid-cols-3 gap-4">
          {filteredDeals.map((deal) => (
            <Card key={deal.id} className={`rounded-2xl shadow-sm ${stageColors[deal.stage]}`}>
              <CardContent className="p-4 space-y-2">
                <div className="text-xs text-gray-500">{deal.id}</div>
                <div className="text-lg font-medium">{deal.customer}</div>
                <div className="text-sm">国：{deal.country}</div>
                <div className="text-sm">製品：{deal.product}</div>
                <div className="text-sm">ステージ：{deal.stage}</div>
                <div className="text-sm flex items-center gap-1">
                  <Factory size={14} /> 工厂状态：{deal.factoryStatus}
                </div>
                <div className="border-t pt-2 text-sm text-blue-600 flex items-center gap-1">
                  <CalendarClock size={14} /> 次の対応：{deal.nextAction}
                </div>
                <div className="text-sm">期限：{deal.nextDate}</div>
              </CardContent>
            </Card>
          ))}

          <Card className="flex items-center justify-center rounded-2xl border-dashed">
            <Button variant="ghost" className="flex items-center gap-2">
              <Plus size={16} /> 新規商談追加
            </Button>
          </Card>
        </div>
      </div>

      {/* 第三排：说明 */}
      <div className="text-xs text-gray-400">
        ③ 色分けは営業ステージを表し、下段は次のアクションと期限を示します
      </div>
    </div>
  );
}
