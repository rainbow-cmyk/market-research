import streamlit as st
import pandas as pd
import random
from datetime import datetime

# ---------- 模拟数据生成逻辑 ----------
def generate_trends(product):
    trends = [
        f"过去3个月，'{product}' 在社交媒体上的讨论量增长了{random.randint(15, 80)}%。",
        f"搜索指数显示，'{product}' 在二线城市的需求环比上升{random.randint(10, 50)}%。",
        f"技术社区关于'{product}' 的开源项目数量增加了{random.randint(5, 30)}个。",
        f"短视频平台 '#{product}评测' 话题播放量突破{random.randint(50, 500)}万次。",
        f"电商平台 '{product}' 相关长尾搜索词增长{random.randint(20, 100)}%。"
    ]
    return random.sample(trends, 3)

def generate_competitors(product):
    comp_names = ["智研X", "快析宝", "DataMind", "TrendMaster", "InsightBot"]
    comp_data = []
    for name in random.sample(comp_names, 3):
        action = random.choice([
            f"最近发布了 '{product}' 实时看板' 功能",
            f"融资{random.randint(100, 500)}万美元加速{product}领域布局",
            f"用户量季度增长{random.randint(20, 80)}%",
            f"推出免费版抢占{product}长尾市场",
            f"与头部咨询公司达成数据合作"
        ])
        comp_data.append({"竞品名称": name, "最新动向": action, "威胁等级": random.choice(["高", "中", "低"])})
    return pd.DataFrame(comp_data)

def generate_pain_points(product):
    points = [
        f"用户普遍反映 '{product}' 现有工具学习成本高，希望有更直观的引导。",
        f"中小团队反馈 '{product}' 定制化能力不足，无法适配特定行业指标。",
        f"多份评论指出 '{product}' 实时性差，数据更新延迟超过24小时。",
        f"用户期待 '{product}' 能自动生成自然语言解读，而非仅展示图表。",
        f"隐私担忧：43%的调研对象不希望 '{product}' 上传原始业务数据。"
    ]
    return random.sample(points, 3)

def generate_marketing_insights(product):
    insights = [
        f"• 内容营销建议：针对 '{product}' 的入门教程和行业案例具有高流量潜力。",
        f"• 未被满足需求：轻量级、无需代码的 '{product}' 解决方案存在蓝海机会。",
        f"• 定价策略：SMB市场对月费低于$99的 '{product}' 工具接受度最高。",
        f"• 渠道侧重：行业垂直社区和知乎/小红书种草效果优于传统广告。"
    ]
    return random.sample(insights, 2)

# ---------- 页面配置 ----------
st.set_page_config(page_title="全自动市场调研助手", page_icon="📊", layout="wide")

st.markdown("""
# 🤖 全自动市场调研助手

**输入任意产品/服务名称**，AI将模拟生成：
- 📈 **全网趋势**：讨论热度、搜索变化、技术生态
- 🧩 **竞品动向**：主要对手的最新动作与威胁评估
- 💡 **用户痛点**：真实反馈与未满足需求
- 🚀 **机会建议**：可落地的营销与产品策略
""")

# ---------- 输入区域 ----------
product = st.text_input("产品名称", placeholder="例如：智能客服机器人、低代码数据看板、AI写作助手...")

# 示例按钮
st.markdown("**试试这些示例：**")
cols = st.columns(4)
examples = ["AI绘画工具", "跨境电商选品SaaS", "企业知识库软件", "家庭健身镜"]
for i, example in enumerate(examples):
    if cols[i].button(example):
        product = example
        st.rerun()

# ---------- 生成报告 ----------
if st.button("🔍 生成调研报告", type="primary"):
    if not product or product.strip() == "":
        st.warning("⚠️ 请输入有效的产品名称。")
    else:
        with st.spinner("正在分析市场数据..."):
            import time
            time.sleep(1.5)
            
            trends = generate_trends(product)
            comp_df = generate_competitors(product)
            pain_points = generate_pain_points(product)
            insights = generate_marketing_insights(product)
            timestamp = datetime.now().strftime("%Y-%m-%d %H:%M:%S")
            
            st.success(f"✅ 报告生成完成！")
            
            st.markdown(f"""
            ---
            
            # 📊 全自动市场调研报告
            
            **产品名称：** {product}  
            **生成时间：** {timestamp}  
            **报告类型：** 全网趋势 · 竞品动向 · 用户痛点
            
            ---
            
            ## 1️⃣ 全网趋势分析
            """)
            
            for t in trends:
                st.markdown(f"- {t}")
            
            st.markdown("## 2️⃣ 重点竞品动向")
            st.dataframe(comp_df, use_container_width=True, hide_index=True)
            
            st.markdown("## 3️⃣ 核心用户痛点")
            for p in pain_points:
                st.markdown(f"- {p}")
            
            st.markdown("## 4️⃣ 市场机会与建议")
            for ins in insights:
                st.markdown(ins)
            
            st.markdown("""
            ---
            *🔍 免责声明：本报告由AI模拟生成，数据基于公开信息聚合与趋势推断，仅供参考。*
            """)
