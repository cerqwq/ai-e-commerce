"""
AI Domain Driven - AI领域驱动设计工具
支持DDD建模、聚合设计、限界上下文
"""

import json
import os
from typing import Dict, List, Any
from datetime import datetime

try:
    from openai import OpenAI
    OPENAI_AVAILABLE = True
except ImportError:
    OPENAI_AVAILABLE = False


class AIDomainDrivenTools:
    """
    AI领域驱动设计工具
    支持：建模、聚合、上下文
    """

    def __init__(self, model: str = "mimo-v2.5-pro", api_key: str = None, base_url: str = None):
        self.model = model
        if OPENAI_AVAILABLE:
            self.client = OpenAI(
                api_key=api_key or os.environ.get('OPENAI_API_KEY', ''),
                base_url=base_url or os.environ.get('OPENAI_BASE_URL', 'https://api.xiaomimimo.com/v1')
            )
        else:
            self.client = None

    def design_domain_model(self, domain: str, requirements: str) -> Dict:
        """设计领域模型"""
        if not self.client:
            return {"error": "LLM客户端未配置"}

        prompt = f"""请为{domain}设计领域模型：

需求：{requirements}

请返回JSON格式：
{{
    "entities": ["实体"],
    "value_objects": ["值对象"],
    "aggregates": ["聚合"],
    "repositories": ["仓储"],
    "domain_events": ["领域事件"]
}}"""

        response = self.client.chat.completions.create(
            model=self.model,
            messages=[{"role": "user", "content": prompt}],
            max_tokens=500
        )

        try:
            content = response.choices[0].message.content
            import re
            json_match = re.search(r'\{.*\}', content, re.DOTALL)
            if json_match:
                return json.loads(json_match.group())
        except:
            pass

        return {"domain": content}

    def design_aggregate(self, aggregate_name: str, entities: List[str]) -> Dict:
        """设计聚合"""
        if not self.client:
            return {"error": "LLM客户端未配置"}

        entities_text = ", ".join(entities)

        prompt = f"""请设计{aggregate_name}聚合：

实体：{entities_text}

请返回JSON格式：
{{
    "root": "聚合根",
    "entities": ["实体"],
    "value_objects": ["值对象"],
    "invariants": ["不变量"],
    "events": ["事件"]
}}"""

        response = self.client.chat.completions.create(
            model=self.model,
            messages=[{"role": "user", "content": prompt}],
            max_tokens=500
        )

        try:
            content = response.choices[0].message.content
            import re
            json_match = re.search(r'\{.*\}', content, re.DOTALL)
            if json_match:
                return json.loads(json_match.group())
        except:
            pass

        return {"aggregate": content}

    def design_bounded_context(self, domain: str, subdomains: List[str]) -> Dict:
        """设计限界上下文"""
        if not self.client:
            return {"error": "LLM客户端未配置"}

        subdomains_text = ", ".join(subdomains)

        prompt = f"""请为{domain}设计限界上下文：

子域：{subdomains_text}

请返回JSON格式：
{{
    "contexts": [
        {{"name": "上下文名", "domain": "领域", "entities": ["实体"]}}
    ],
    "relationships": ["上下文映射"],
    "integration": "集成策略"
}}"""

        response = self.client.chat.completions.create(
            model=self.model,
            messages=[{"role": "user", "content": prompt}],
            max_tokens=500
        )

        try:
            content = response.choices[0].message.content
            import re
            json_match = re.search(r'\{.*\}', content, re.DOTALL)
            if json_match:
                return json.loads(json_match.group())
        except:
            pass

        return {"bounded_context": content}

    def generate_domain_event(self, event_name: str, payload: Dict) -> str:
        """生成领域事件"""
        if not self.client:
            return "LLM客户端未配置"

        payload_text = json.dumps(payload, ensure_ascii=False)

        prompt = f"""请生成{event_name}领域事件：

数据：{payload_text}

要求：
1. 事件类定义
2. 序列化支持
3. 版本控制"""

        response = self.client.chat.completions.create(
            model=self.model,
            messages=[{"role": "user", "content": prompt}],
            max_tokens=1500
        )

        return response.choices[0].message.content

    def generate_repository(self, aggregate: str, db_type: str = "SQLAlchemy") -> str:
        """生成仓储实现"""
        if not self.client:
            return "LLM客户端未配置"

        prompt = f"""请生成{aggregate}的{db_type}仓储实现：

要求：
1. CRUD操作
2. 查询支持
3. 事务管理
4. 错误处理"""

        response = self.client.chat.completions.create(
            model=self.model,
            messages=[{"role": "user", "content": prompt}],
            max_tokens=2000
        )

        return response.choices[0].message.content

    def analyze_domain_complexity(self, domain_description: str) -> Dict:
        """分析领域复杂度"""
        if not self.client:
            return {"error": "LLM客户端未配置"}

        prompt = f"""请分析领域复杂度：

{domain_description[:1000]}

请返回JSON格式：
{{
    "complexity": "high/medium/low",
    "core_domain": "核心域",
    "supporting_domains": ["支撑域"],
    "generic_domains": ["通用域"],
    "recommendations": ["建议"]
}}"""

        response = self.client.chat.completions.create(
            model=self.model,
            messages=[{"role": "user", "content": prompt}],
            max_tokens=500
        )

        try:
            content = response.choices[0].message.content
            import re
            json_match = re.search(r'\{.*\}', content, re.DOTALL)
            if json_match:
                return json.loads(json_match.group())
        except:
            pass

        return {"analysis": content}


def create_tools(**kwargs) -> AIDomainDrivenTools:
    """创建DDD工具"""
    return AIDomainDrivenTools(**kwargs)


if __name__ == "__main__":
    tools = create_tools()

    print("AI Domain Driven Tools")
    print()

    # 测试
    domain = tools.design_domain_model("电商", "用户、商品、订单、支付")
    print(json.dumps(domain, ensure_ascii=False, indent=2))
