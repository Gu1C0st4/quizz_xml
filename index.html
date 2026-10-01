/* =========================================================================
   XML & XSD — aula interativa
   Snippets, syntax highlight, validador XML↔XSD (browser), sandbox e quiz.
   ========================================================================= */

/* ---------- Snippets de código (estáticos) ---------- */
const SNIPS = {
  hero:
`<?xml version="1.0" encoding="UTF-8"?>
<estacao id="PT-VIS-07">
  <nome>Rossio</nome>
  <bicicletas disponiveis="12"/>
</estacao>`,
  anatomia:
`<?xml version="1.0" encoding="UTF-8"?>
<rede atualizadaEm="2026-09-25T09:30:00Z">
  <estacao id="PT-VIS-07">
    <nome>Rossio</nome>
    <localizacao lat="40.6575" lon="-7.9139"/>
    <bicicletas disponiveis="12"/>
  </estacao>
</rede>`,
  erros:
`<rede atualizadaEm=2026-09-25>
  <estacao id="PT-VIS-07">
    <nome>Rossio</Nome>
    <bicicletas disponiveis="12">
  </estacao>
</rede>
<estado>operacional</estado>`,
  corrigido:
`<rede atualizadaEm="2026-09-25">
  <estacao id="PT-VIS-07">
    <nome>Rossio</nome>
    <bicicletas disponiveis="12"/>
    <estado>operacional</estado>
  </estacao>
</rede>`,
  ligacao:
`<!-- no XML -->
<rede xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xsi:noNamespaceSchemaLocation="mobilidade.xsd">
  ...
</rede>

<!-- no XSD -->
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  ...
</xs:schema>`,
  restricao:
`<xs:simpleType name="DisponibilidadeType">
  <xs:restriction base="xs:nonNegativeInteger">
    <xs:maxInclusive value="80"/>
  </xs:restriction>
</xs:simpleType>

<xs:attribute name="disponiveis"
    type="DisponibilidadeType" use="required"/>`,
  complexo:
`<xs:complexType name="EstacaoType">
  <xs:sequence>
    <xs:element name="nome" type="xs:string"/>
    <xs:element name="localizacao" type="LocalizacaoType"/>
    <xs:element name="bicicletas" type="BicicletasType"/>
    <xs:element name="estado" type="EstadoType"/>
  </xs:sequence>
  <xs:attribute name="id" type="EstacaoIdType" use="required"/>
</xs:complexType>`
};

/* ---------- Syntax highlight (XML) ---------- */
function esc(s){ return s.replace(/&/g,"&amp;").replace(/</g,"&lt;").replace(/>/g,"&gt;"); }
function highlightXml(code){
  let s = esc(code);
  // 1) atributos primeiro (no texto puro, antes de inserir quaisquer <span>)
  s = s.replace(/([A-Za-z_][\w:.\-]*)=(")([^"]*)(")/g,
        '<span class="hl-attr">$1</span>=<span class="hl-str">$2$3$4</span>');
  // 2) comentários
  s = s.replace(/(&lt;!--[\s\S]*?--&gt;)/g, '<span class="hl-com">$1</span>');
  // 3) declaração
  s = s.replace(/(&lt;\?[\s\S]*?\?&gt;)/g, '<span class="hl-decl">$1</span>');
  // 4) etiquetas (os <span> já inseridos usam "<" real, não "&lt;", por isso não são afetados)
  s = s.replace(/(&lt;\/?)([A-Za-z_][\w:.\-]*)/g, '$1<span class="hl-tag">$2</span>');
  return s;
}
function paintSnippets(){
  document.querySelectorAll('pre.code[data-snip]').forEach(pre=>{
    const key = pre.getAttribute('data-snip');
    if(!SNIPS[key]) return;
    const bar = pre.querySelector('.bar');
    pre.innerHTML = (bar ? bar.outerHTML : '') + highlightXml(SNIPS[key]);
  });
}

/* =========================================================================
   VALIDADOR XML ↔ XSD
   ========================================================================= */
const XSD_NS = "http://www.w3.org/2001/XMLSchema";
const PRIMS = new Set([
  "string","normalizedString","token","language","anyURI","QName",
  "integer","int","long","short","byte","nonNegativeInteger","positiveInteger",
  "nonPositiveInteger","negativeInteger","unsignedInt","unsignedLong",
  "decimal","double","float","boolean","date","time","dateTime","duration",
  "gYear","gMonth","gDay","gYearMonth","gMonthDay","ID","IDREF","NMTOKEN",
  "base64Binary","hexBinary","anyType"
]);
const stripPrefix = q => (q||"").includes(":") ? q.split(":").pop() : (q||"");
const isPrimitive = n => PRIMS.has(n);
const elChildren = n => Array.from(n.children);
const xsdKids = n => elChildren(n).filter(c => c.localName);
const childByLocal = (n,ln) => xsdKids(n).find(c => c.localName===ln) || null;
const textOf = n => Array.from(n.childNodes).filter(x=>x.nodeType===3).map(x=>x.nodeValue).join("");

function parseXml(str){
  const doc = new DOMParser().parseFromString(str, "application/xml");
  const err = doc.getElementsByTagName("parsererror");
  if(err.length){
    let msg = err[0].textContent.replace(/\s+/g," ").trim();
    return { ok:false, error: msg };
  }
  if(!doc.documentElement) return { ok:false, error:"Documento vazio." };
  return { ok:true, doc };
}

function buildSchema(xsdDoc){
  const s = { elements:{}, simpleTypes:{}, complexTypes:{}, attributes:{}, groups:{}, attributeGroups:{} };
  const root = xsdDoc.documentElement;
  for(const c of xsdKids(root)){
    const nm = c.getAttribute("name");
    switch(c.localName){
      case "element":        if(nm) s.elements[nm]=c; break;
      case "simpleType":     if(nm) s.simpleTypes[nm]=c; break;
      case "complexType":    if(nm) s.complexTypes[nm]=c; break;
      case "attribute":      if(nm) s.attributes[nm]=c; break;
      case "group":          if(nm) s.groups[nm]=c; break;
      case "attributeGroup": if(nm) s.attributeGroups[nm]=c; break;
    }
  }
  return s;
}

/* ---- content model → particle tree ---- */
function firstCompositor(n, schema){
  return childByLocal(n,"sequence") || childByLocal(n,"choice") || childByLocal(n,"all") ||
         (childByLocal(n,"group") ? childByLocal(n,"group") : null);
}
function occNums(node){
  const min = node.hasAttribute("minOccurs") ? parseInt(node.getAttribute("minOccurs"),10) : 1;
  const mr = node.getAttribute("maxOccurs");
  const max = mr==null ? 1 : (mr==="unbounded" ? Infinity : parseInt(mr,10));
  return { min:isNaN(min)?1:min, max:isNaN(max)?1:max };
}
function parseParticle(node, schema){
  const ln = node.localName;
  const { min, max } = occNums(node);
  if(ln==="element"){
    const name = node.getAttribute("name") || stripPrefix(node.getAttribute("ref")||"");
    return { type:"element", name, decl:node, min, max };
  }
  if(ln==="any"){ return { type:"any", min, max }; }
  if(ln==="group"){
    const ref = node.getAttribute("ref");
    const g = ref ? schema.groups[stripPrefix(ref)] : node;
    const inner = g ? firstCompositor(g, schema) : null;
    return { type:"group", min, max, particle: inner ? parseParticle(inner, schema) : null };
  }
  // sequence | choice | all
  const parts = xsdKids(node)
    .filter(c => ["element","sequence","choice","all","group","any"].includes(c.localName))
    .map(c => parseParticle(c, schema));
  return { type:ln, min, max, parts };
}

/* ---- matcher: returns Set of reachable end indices ---- */
function occ(start, min, max, stepFn, blockFn){
  const results = new Set();
  if(min<=0) results.add(start);
  let frontier = new Set([start]);
  const seen = new Set([start]);
  let count = 0;
  while(count < max){
    const nf = new Set();
    for(const pos of frontier){
      if(stepFn){ const np = stepFn(pos); if(np!=null) nf.add(np); }
      else { for(const e of blockFn(pos)) nf.add(e); }
    }
    count++;
    if(nf.size===0) break;
    let progressed = false;
    for(const e of nf){
      if(count>=min) results.add(e);
      if(!seen.has(e)){ seen.add(e); progressed=true; }
    }
    frontier = nf;
    if(!progressed) break; // positions are bounded → fixpoint reached
  }
  return results;
}
function matchParticle(p, children, start){
  if(!p) return new Set([start]);
  if(p.type==="element"){
    return occ(start, p.min, p.max, pos =>
      (pos<children.length && children[pos].localName===p.name) ? pos+1 : null);
  }
  if(p.type==="any"){
    return occ(start, p.min, p.max, pos => pos<children.length ? pos+1 : null);
  }
  if(p.type==="group"){
    return occ(start, p.min, p.max, null, pos => matchParticle(p.particle, children, pos));
  }
  if(p.type==="sequence"){
    const once = pos => {
      let cur = new Set([pos]);
      for(const sub of p.parts){
        const nx = new Set();
        for(const c of cur) for(const e of matchParticle(sub, children, c)) nx.add(e);
        if(nx.size===0) return new Set();
        cur = nx;
      }
      return cur;
    };
    return occ(start, p.min, p.max, null, once);
  }
  if(p.type==="choice"){
    const once = pos => {
      const r = new Set();
      for(const sub of p.parts) for(const e of matchParticle(sub, children, pos)) r.add(e);
      return r;
    };
    return occ(start, p.min, p.max, null, once);
  }
  if(p.type==="all"){
    const byName = {}; p.parts.forEach(pp => { if(pp.type==="element") byName[pp.name]=pp; });
    const used = {}; let pos = start;
    while(pos<children.length){
      const pp = byName[children[pos].localName];
      if(!pp) break;
      used[pp.name] = (used[pp.name]||0)+1;
      if(used[pp.name] > (pp.max||1)) break;
      pos++;
    }
    const ok = p.parts.every(pp => pp.type!=="element" || (pp.min||1)===0 || used[pp.name]);
    const res = new Set();
    if(ok) res.add(pos);
    if((p.min||1)===0) res.add(start);
    return res;
  }
  return new Set([start]);
}
function collectElementDecls(p, map){
  if(!p) return;
  if(p.type==="element"){ if(!map[p.name]) map[p.name]=p.decl; }
  else if(p.parts){ p.parts.forEach(x=>collectElementDecls(x,map)); }
  else if(p.particle){ collectElementDecls(p.particle,map); }
}
function expectedNames(p, acc){
  if(!p) return;
  if(p.type==="element"){ acc.push(p.name); }
  else if(p.parts){ p.parts.forEach(x=>expectedNames(x,acc)); }
  else if(p.particle){ expectedNames(p.particle,acc); }
}

/* ---- attributes ---- */
function collectAttributes(container, schema){
  let res = [];
  if(!container) return res;
  for(const c of xsdKids(container)){
    if(c.localName==="attribute"){
      if(c.getAttribute("ref")){
        const a = schema.attributes[stripPrefix(c.getAttribute("ref"))];
        if(a) res.push({ node:a, name:a.getAttribute("name"), use:c.getAttribute("use")||a.getAttribute("use")||"optional" });
      } else {
        res.push({ node:c, name:c.getAttribute("name"), use:c.getAttribute("use")||"optional" });
      }
    } else if(c.localName==="attributeGroup" && c.getAttribute("ref")){
      const g = schema.attributeGroups[stripPrefix(c.getAttribute("ref"))];
      if(g) res = res.concat(collectAttributes(g, schema));
    }
  }
  return res;
}

/* ---- simple-type resolution + facets ---- */
function simpleInfoFromType(typeAttr, inlineNode, schema){
  if(typeAttr){
    const tn = stripPrefix(typeAttr);
    if(isPrimitive(tn)) return { base:tn, node:null };
    if(schema.simpleTypes[tn]) return { base:tn, node:schema.simpleTypes[tn] };
    return { base:"string", node:null, unknown:tn };
  }
  const st = inlineNode ? childByLocal(inlineNode,"simpleType") : null;
  if(st) return { base:"anyType", node:st };
  return { base:"string", node:null };
}
function resolveSimple(info, schema){
  let base = info.base || "string";
  let node = info.node || null;
  const facets = [];
  let guard = 0;
  while(node && guard++ < 30){
    const r = childByLocal(node,"restriction");
    if(!r) break;
    const b = stripPrefix(r.getAttribute("base")||"string");
    for(const f of xsdKids(r)){
      if(f.localName) facets.push({ kind:f.localName, value:f.getAttribute("value") });
    }
    if(isPrimitive(b)){ base = b; node = null; }
    else if(schema.simpleTypes[b]){ base = b; node = schema.simpleTypes[b]; }
    else { base = b; node = null; }
  }
  return { base, facets };
}
function checkBase(base, v){
  switch(base){
    case "integer": case "int": case "long": case "short": case "byte":
      return /^[+-]?\d+$/.test(v);
    case "nonNegativeInteger": case "unsignedInt": case "unsignedLong":
      return /^\+?\d+$/.test(v);
    case "positiveInteger":
      return /^\+?\d+$/.test(v) && Number(v)>0;
    case "nonPositiveInteger":
      return /^-?\d+$/.test(v) && Number(v)<=0;
    case "negativeInteger":
      return /^-\d+$/.test(v) && Number(v)<0;
    case "decimal": case "double": case "float":
      return /^[+-]?(\d+\.?\d*|\.\d+)([eE][+-]?\d+)?$/.test(v);
    case "boolean":
      return /^(true|false|0|1)$/.test(v);
    case "date":
      return /^-?\d{4}-\d{2}-\d{2}(Z|[+-]\d{2}:\d{2})?$/.test(v);
    case "time":
      return /^\d{2}:\d{2}:\d{2}(\.\d+)?(Z|[+-]\d{2}:\d{2})?$/.test(v);
    case "dateTime":
      return /^-?\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}(\.\d+)?(Z|[+-]\d{2}:\d{2})?$/.test(v);
    case "gYear":
      return /^-?\d{4}$/.test(v);
    default:
      return true; // string-like / anyType
  }
}
function baseLabel(b){ return "xs:"+b; }
function validateSimpleValue(rawValue, info, schema, errors, path, node){
  const add = msg => errors.push({ msg, node });
  const { base, facets } = resolveSimple(info, schema);
  const v = (rawValue==null ? "" : String(rawValue)).trim();
  if(!checkBase(base, v)){
    add(`${path}: o valor "${v||"(vazio)"}" não é um ${baseLabel(base)} válido.`);
    return;
  }
  const enums = facets.filter(f=>f.kind==="enumeration").map(f=>f.value);
  if(enums.length && !enums.includes(v)){
    add(`${path}: "${v}" não está entre os valores permitidos (${enums.join(", ")}).`);
  }
  const patterns = facets.filter(f=>f.kind==="pattern").map(f=>f.value);
  if(patterns.length){
    const ok = patterns.some(p=>{ try{ return new RegExp("^(?:"+p+")$").test(v); }catch(e){ return true; } });
    if(!ok) add(`${path}: "${v}" não corresponde ao padrão ${patterns.join(" | ")}.`);
  }
  const num = Number(v);
  for(const f of facets){
    const fv = f.value;
    switch(f.kind){
      case "minInclusive": if(num <  Number(fv)) add(`${path}: ${v} tem de ser ≥ ${fv}.`); break;
      case "maxInclusive": if(num >  Number(fv)) add(`${path}: ${v} tem de ser ≤ ${fv}.`); break;
      case "minExclusive": if(num <= Number(fv)) add(`${path}: ${v} tem de ser > ${fv}.`); break;
      case "maxExclusive": if(num >= Number(fv)) add(`${path}: ${v} tem de ser < ${fv}.`); break;
      case "length":    if(v.length !== +fv) add(`${path}: deve ter exatamente ${fv} caracteres.`); break;
      case "minLength": if(v.length <  +fv) add(`${path}: deve ter no mínimo ${fv} caracteres.`); break;
      case "maxLength": if(v.length >  +fv) add(`${path}: deve ter no máximo ${fv} caracteres.`); break;
    }
  }
}
function validateAttributes(node, attrDecls, schema, errors, path){
  const byName = {}; attrDecls.forEach(a=>byName[a.name]=a);
  for(const attr of Array.from(node.attributes)){
    if(attr.name==="xmlns" || attr.prefix==="xmlns" || attr.prefix==="xsi" || attr.name==="xsi") continue;
    const an = attr.localName;
    const d = byName[an];
    if(!d){ errors.push({ msg:`${path}: o atributo "${an}" não está declarado no XSD.`, node }); continue; }
    const info = simpleInfoFromType(d.node.getAttribute("type"), d.node, schema);
    validateSimpleValue(attr.value, info, schema, errors, `${path} @${an}`, node);
    const fixed = d.node.getAttribute("fixed");
    if(fixed!=null && attr.value!==fixed) errors.push({ msg:`${path}: o atributo "${an}" é fixo e tem de ser "${fixed}".`, node });
  }
  for(const a of attrDecls){
    if(a.use==="required" && !node.getAttribute(a.name)){
      errors.push({ msg:`${path}: falta o atributo obrigatório "${a.name}".`, node });
    }
  }
}

/* ---- element / complex-type validation ---- */
function resolveElementDecl(decl, schema){
  if(decl.getAttribute && decl.getAttribute("ref")){
    return schema.elements[stripPrefix(decl.getAttribute("ref"))] || decl;
  }
  return decl;
}
function getType(decl, schema){
  const t = decl.getAttribute("type");
  if(t){
    const tn = stripPrefix(t);
    if(isPrimitive(tn)) return { kind:"simple", primitive:tn };
    if(schema.simpleTypes[tn]) return { kind:"simple", node:schema.simpleTypes[tn], named:tn };
    if(schema.complexTypes[tn]) return { kind:"complex", node:schema.complexTypes[tn], named:tn };
    return { kind:"simple", primitive:"string", unknown:tn };
  }
  const ct = childByLocal(decl,"complexType");
  if(ct) return { kind:"complex", node:ct };
  const st = childByLocal(decl,"simpleType");
  if(st) return { kind:"simple", node:st };
  return { kind:"none" };
}
function validateElement(node, decl, schema, errors, path){
  decl = resolveElementDecl(decl, schema);
  const ti = getType(decl, schema);
  if(ti.kind==="complex"){
    validateComplex(node, ti.node, schema, errors, path);
  } else if(ti.kind==="simple"){
    if(elChildren(node).length){
      errors.push({ msg:`${path}: esperava texto simples, mas encontrou elementos filhos.`, node });
    }
    if(ti.node) validateSimpleValue(textOf(node), { base:"anyType", node:ti.node }, schema, errors, path, node);
    else validateSimpleValue(textOf(node), { base:ti.primitive||"string", node:null }, schema, errors, path, node);
  }
}
function validateComplex(node, ct, schema, errors, path){
  if(!ct){ return; }
  const simpleContent = childByLocal(ct,"simpleContent");
  if(simpleContent){
    const ext = childByLocal(simpleContent,"extension") || childByLocal(simpleContent,"restriction");
    const base = ext ? stripPrefix(ext.getAttribute("base")||"string") : "string";
    validateSimpleValue(textOf(node), { base:isPrimitive(base)?base:"string", node:schema.simpleTypes[base]||null }, schema, errors, path, node);
    validateAttributes(node, collectAttributes(ext, schema), schema, errors, path);
    if(elChildren(node).length) errors.push({ msg:`${path}: simpleContent não permite elementos filhos.`, node });
    return;
  }
  const complexContent = childByLocal(ct,"complexContent");
  if(complexContent){
    const ext = childByLocal(complexContent,"extension");
    if(ext){
      const baseCt = schema.complexTypes[stripPrefix(ext.getAttribute("base")||"")];
      const parts = [];
      if(baseCt){ const bc = firstCompositor(baseCt, schema); if(bc) parts.push(parseParticle(bc, schema)); }
      const ec = firstCompositor(ext, schema); if(ec) parts.push(parseParticle(ec, schema));
      const combined = { type:"sequence", min:1, max:1, parts };
      runContentModel(node, combined, schema, errors, path, ct.getAttribute("mixed")==="true");
      const attrs = (baseCt?collectAttributes(baseCt,schema):[]).concat(collectAttributes(ext,schema));
      validateAttributes(node, attrs, schema, errors, path);
      return;
    }
  }
  const compositorNode = firstCompositor(ct, schema);
  const particle = compositorNode ? parseParticle(compositorNode, schema) : null;
  runContentModel(node, particle, schema, errors, path, ct.getAttribute("mixed")==="true");
  validateAttributes(node, collectAttributes(ct, schema), schema, errors, path);
}
function runContentModel(node, particle, schema, errors, path, mixed){
  const children = elChildren(node);
  if(!particle){
    if(children.length) errors.push({ msg:`${path}: não são permitidos elementos filhos aqui.`, node });
    return;
  }
  const ends = matchParticle(particle, children, 0);
  if(!ends.has(children.length)){
    const exp = []; expectedNames(particle, exp);
    const got = children.map(c=>c.localName);
    errors.push({ msg:`${path}: a ordem/número de filhos não corresponde ao esquema. Esperado: [${[...new Set(exp)].join(", ")}] · Encontrado: [${got.join(", ")||"vazio"}].`, node });
  }
  const map = {}; collectElementDecls(particle, map);
  for(const child of children){
    const d = map[child.localName];
    if(d) validateElement(child, d, schema, errors, `${path} › ${child.localName}`);
  }
}

/* ---- mapa de linhas (nó → linha no texto fonte) ---- */
function countNL(str){ let n=0; for(let i=0;i<str.length;i++) if(str[i]==='\n') n++; return n; }
function startTagLines(s){
  const res = []; let line = 1; let i = 0;
  while(i < s.length){
    if(s[i] === '\n'){ line++; i++; continue; }
    if(s[i] === '<'){
      if(s.startsWith('<!--', i)){ const e=s.indexOf('-->', i+4); const seg=s.slice(i, e<0?s.length:e+3); line+=countNL(seg); i=(e<0?s.length:e+3); continue; }
      if(s.startsWith('<![CDATA[', i)){ const e=s.indexOf(']]>', i+9); const seg=s.slice(i, e<0?s.length:e+3); line+=countNL(seg); i=(e<0?s.length:e+3); continue; }
      if(s[i+1] === '?'){ const e=s.indexOf('?>', i+2); const seg=s.slice(i, e<0?s.length:e+2); line+=countNL(seg); i=(e<0?s.length:e+2); continue; }
      if(s[i+1] === '!'){ const e=s.indexOf('>', i+2); const seg=s.slice(i, e<0?s.length:e+1); line+=countNL(seg); i=(e<0?s.length:e+1); continue; }
      if(s[i+1] === '/'){ i+=2; continue; }
      if(/[A-Za-z_]/.test(s[i+1] || '')){ res.push(line); i++; continue; }
    }
    i++;
  }
  return res;
}
function assignLines(xmlDoc, xmlStr){
  const lines = startTagLines(xmlStr);
  const map = new WeakMap();
  let idx = 0;
  (function walk(node){
    map.set(node, lines[idx] != null ? lines[idx] : null);
    idx++;
    for(const c of elChildren(node)) walk(c);
  })(xmlDoc.documentElement);
  return map;
}

/* ---- orchestration ---- */
function validateXmlAgainstXsd(xmlStr, xsdStr){
  const out = { steps:{xmlWF:false, xsdWF:false, compiled:false, valid:false}, errors:[], fatal:null, xml:xmlStr, errorLines:new Set() };

  const xml = parseXml(xmlStr);
  if(!xml.ok){
    const m = /line (\d+)/i.exec(xml.error);
    if(m) out.errorLines.add(+m[1]);
    out.fatal = "O XML não está bem formado: " + xml.error;
    return out;
  }
  out.steps.xmlWF = true;

  const xsd = parseXml(xsdStr);
  if(!xsd.ok){ out.fatal = "O XSD não está bem formado: " + xsd.error; return out; }
  out.steps.xsdWF = true;

  let schema;
  try { schema = buildSchema(xsd.doc); out.steps.compiled = true; }
  catch(e){ out.fatal = "Não foi possível compilar o esquema: " + e.message; return out; }

  const rootEl = xml.doc.documentElement;
  const decl = schema.elements[rootEl.localName];
  if(!decl){
    out.errors.push({ msg:`O elemento raiz <${rootEl.localName}> não está declarado como elemento global no XSD. Globais: [${Object.keys(schema.elements).join(", ")||"nenhum"}].`, node:rootEl });
  } else {
    try {
      validateElement(rootEl, decl, schema, out.errors, rootEl.localName);
    } catch(e){
      out.errors.push({ msg:"Erro inesperado durante a validação: " + e.message, node:rootEl });
    }
  }

  // atribuir linhas aos erros
  try {
    const lineMap = assignLines(xml.doc, xmlStr);
    out.errors.forEach(e=>{
      e.line = e.node ? (lineMap.get(e.node) || null) : null;
      if(e.line) out.errorLines.add(e.line);
    });
  } catch(e){ /* sem linhas, mas mantém as mensagens */ }

  out.steps.valid = out.errors.length===0;
  return out;
}

/* =========================================================================
   PRESETS (exemplos para o sandbox)
   ========================================================================= */
const REDE_XSD =
`<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">

  <xs:simpleType name="EstacaoIdType">
    <xs:restriction base="xs:string">
      <xs:pattern value="PT-[A-Z]{3}-[0-9]{2}"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="EstadoType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="operacional"/>
      <xs:enumeration value="manutencao"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:simpleType name="DisponibilidadeType">
    <xs:restriction base="xs:nonNegativeInteger">
      <xs:maxInclusive value="80"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:complexType name="LocalizacaoType">
    <xs:attribute name="lat" type="xs:decimal" use="required"/>
    <xs:attribute name="lon" type="xs:decimal" use="required"/>
  </xs:complexType>

  <xs:complexType name="BicicletasType">
    <xs:attribute name="disponiveis" type="DisponibilidadeType" use="required"/>
  </xs:complexType>

  <xs:complexType name="EstacaoType">
    <xs:sequence>
      <xs:element name="nome" type="xs:string"/>
      <xs:element name="localizacao" type="LocalizacaoType"/>
      <xs:element name="bicicletas" type="BicicletasType"/>
      <xs:element name="estado" type="EstadoType"/>
    </xs:sequence>
    <xs:attribute name="id" type="EstacaoIdType" use="required"/>
  </xs:complexType>

  <xs:element name="rede">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="estacao" type="EstacaoType" minOccurs="1" maxOccurs="unbounded"/>
      </xs:sequence>
      <xs:attribute name="atualizadaEm" type="xs:dateTime"/>
    </xs:complexType>
  </xs:element>

</xs:schema>`;

function redeXml(estacoes){
  return `<?xml version="1.0" encoding="UTF-8"?>
<rede atualizadaEm="2026-09-25T09:30:00Z">
${estacoes}
</rede>`;
}
const EST_OK_1 =
`  <estacao id="PT-VIS-07">
    <nome>Rossio</nome>
    <localizacao lat="40.6575" lon="-7.9139"/>
    <bicicletas disponiveis="12"/>
    <estado>operacional</estado>
  </estacao>`;
const EST_OK_2 =
`  <estacao id="PT-LIS-01">
    <nome>Se</nome>
    <localizacao lat="38.7101" lon="-9.1333"/>
    <bicicletas disponiveis="4"/>
    <estado>manutencao</estado>
  </estacao>`;

const PRESETS = [
  {
    label:"✓ Rede (válido)",
    xml: redeXml(EST_OK_1 + "\n" + EST_OK_2),
    xsd: REDE_XSD
  },
  {
    label:"✗ Disponibilidade negativa",
    xml: redeXml(
`  <estacao id="PT-VIS-07">
    <nome>Rossio</nome>
    <localizacao lat="40.6575" lon="-7.9139"/>
    <bicicletas disponiveis="-4"/>
    <estado>operacional</estado>
  </estacao>`),
    xsd: REDE_XSD
  },
  {
    label:"✗ id em formato errado",
    xml: redeXml(
`  <estacao id="VIS-7">
    <nome>Rossio</nome>
    <localizacao lat="40.6575" lon="-7.9139"/>
    <bicicletas disponiveis="12"/>
    <estado>operacional</estado>
  </estacao>`),
    xsd: REDE_XSD
  },
  {
    label:"✗ falta elemento obrigatório",
    xml: redeXml(
`  <estacao id="PT-VIS-07">
    <nome>Rossio</nome>
    <localizacao lat="40.6575" lon="-7.9139"/>
    <bicicletas disponiveis="12"/>
  </estacao>`),
    xsd: REDE_XSD
  },
  {
    label:"✗ estado inválido (enum)",
    xml: redeXml(
`  <estacao id="PT-VIS-07">
    <nome>Rossio</nome>
    <localizacao lat="40.6575" lon="-7.9139"/>
    <bicicletas disponiveis="12"/>
    <estado>aberta</estado>
  </estacao>`),
    xsd: REDE_XSD
  },
  {
    label:"✓ Menu do dia (válido)",
    xml:
`<?xml version="1.0" encoding="UTF-8"?>
<menu_do_dia>
  <item regime="Normal">
    <nome>Carne de Porco a Alentejana</nome>
    <descricao>Batata frita com carne de porco</descricao>
    <preco>5.95</preco>
  </item>
  <item regime="Vegetariano">
    <nome>Doce da Casa</nome>
    <descricao>Doce especial da casa</descricao>
    <preco>1.50</preco>
  </item>
</menu_do_dia>`,
    xsd:
`<?xml version="1.0" encoding="UTF-8"?>
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:simpleType name="RegimeType">
    <xs:restriction base="xs:string">
      <xs:enumeration value="Normal"/>
      <xs:enumeration value="Vegetariano"/>
      <xs:enumeration value="Vegan"/>
    </xs:restriction>
  </xs:simpleType>

  <xs:element name="menu_do_dia">
    <xs:complexType>
      <xs:sequence maxOccurs="8">
        <xs:element name="item">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="nome" type="xs:string"/>
              <xs:element name="descricao" type="xs:string"/>
              <xs:element name="preco" type="xs:decimal"/>
            </xs:sequence>
            <xs:attribute name="regime" type="RegimeType" use="required"/>
          </xs:complexType>
        </xs:element>
      </xs:sequence>
    </xs:complexType>
  </xs:element>
</xs:schema>`
  }
];

/* =========================================================================
   SANDBOX wiring
   ========================================================================= */
/* ---- editores com realce de sintaxe (camada colorida sob o textarea) ---- */
const HL_REFRESH = [];
function setupEditor(taId, hlId){
  const ta = document.getElementById(taId);
  const hl = document.getElementById(hlId);
  if(!ta || !hl) return;
  const render = ()=>{ hl.innerHTML = highlightXml(ta.value) + "\n"; };
  const syncScroll = ()=>{ hl.scrollTop = ta.scrollTop; hl.scrollLeft = ta.scrollLeft; };
  ta.addEventListener("input", ()=>{ render(); syncScroll(); });
  ta.addEventListener("scroll", syncScroll);
  ta.addEventListener("keydown", (e)=>{
    if(e.key === "Tab"){
      e.preventDefault();
      const s = ta.selectionStart, en = ta.selectionEnd;
      ta.value = ta.value.slice(0, s) + "  " + ta.value.slice(en);
      ta.selectionStart = ta.selectionEnd = s + 2;
      render(); syncScroll();
    }
  });
  HL_REFRESH.push(()=>{ render(); syncScroll(); });
  render();
}
function refreshEditors(){ HL_REFRESH.forEach(f=>f()); }

function renderResult(out){
  const el = document.getElementById("result");
  const steps = `
    <div class="steps">
      <span class="s ${out.steps.xmlWF?'pass':'fail'}">XML bem formado</span>
      <span class="s ${out.steps.xsdWF?'pass':'fail'}">XSD bem formado</span>
      <span class="s ${out.steps.compiled?'pass':''}">Esquema compilado</span>
      <span class="s ${out.steps.valid?'pass':(out.fatal?'':'fail')}">Instância válida</span>
    </div>`;
  if(out.fatal){
    const view = (out.xml && out.errorLines && out.errorLines.size) ? renderXmlView(out.xml, out.errorLines) : "";
    el.innerHTML = `<div class="res bad">${steps}<h4>⚠ Não foi possível validar</h4>
      <ul class="errlist"><li>${esc(out.fatal)}</li></ul>${view}</div>`;
    return;
  }
  if(out.errors.length===0){
    el.innerHTML = `<div class="res ok">${steps}<h4>✓ Válido</h4>
      <p class="sub">O documento está bem formado e respeita todas as regras do XSD.</p></div>`;
    return;
  }
  const items = out.errors.map(e=>{
    const ln = e.line ? `<span class="eln">Linha ${e.line}</span>` : "";
    return `<li>${ln}${esc(e.msg)}</li>`;
  }).join("");
  const view = renderXmlView(out.xml, out.errorLines);
  el.innerHTML = `<div class="res bad">${steps}<h4>✗ ${out.errors.length} ${out.errors.length===1?'erro':'erros'} de validação</h4>
    <p class="sub">O XML está bem formado, mas viola o contrato definido no XSD:</p>
    <ul class="errlist">${items}</ul>${view}</div>`;
}
function renderXmlView(xmlStr, errorLines){
  if(!xmlStr) return "";
  const lines = xmlStr.replace(/\t/g, "  ").split("\n");
  const rows = lines.map((ln, i)=>{
    const num = i + 1;
    const bad = errorLines && errorLines.has(num);
    const code = highlightXml(ln) || "&nbsp;";
    return `<div class="xrow${bad?' err':''}"><span class="ln">${num}</span><span class="xcode">${code}</span></div>`;
  }).join("");
  return `<div class="xml-view-cap">Documento XML — linhas com erro assinaladas a vermelho:</div>
    <div class="xml-view">${rows}</div>`;
}
function runValidation(){
  const xml = document.getElementById("xmlIn").value;
  const xsd = document.getElementById("xsdIn").value;
  renderResult(validateXmlAgainstXsd(xml, xsd));
}
function loadPreset(i){
  document.getElementById("xmlIn").value = PRESETS[i].xml;
  document.getElementById("xsdIn").value = PRESETS[i].xsd;
  refreshEditors();
  runValidation();
  document.getElementById("result").scrollIntoView({behavior:"smooth", block:"nearest"});
}
function buildPresets(){
  const box = document.getElementById("presets");
  box.innerHTML = PRESETS.map((p,i)=>`<span class="chip" data-i="${i}">${p.label}</span>`).join("");
  box.querySelectorAll(".chip").forEach(c=>c.addEventListener("click",()=>loadPreset(+c.dataset.i)));
}

/* =========================================================================
   CONCEITOS (toggles no glossário)
   ========================================================================= */
const CONCEPTS = [
  { term:"XML", cat:"",
    body:`<p>eXtensible Markup Language — standard da W3C (1998) para estruturar dados de forma hierárquica, legível por humanos e máquinas. Fornece <b>sintaxe</b>, não semântica: as tags só têm o significado que a aplicação lhes der.</p>`,
    code:`<prato_do_dia>\n  <item>\n    <nome>Big Mac</nome>\n  </item>\n</prato_do_dia>` },
  { term:"Elemento", cat:"",
    body:`<p>A unidade básica do XML, delimitada por uma tag de abertura e outra de fecho. Pode conter texto, outros elementos (agregador), ou nada (vazio: <code>&lt;obs/&gt;</code>).</p>` },
  { term:"Atributo", cat:"coral",
    body:`<p>Um par <b>nome=valor</b> dentro da tag de abertura, que qualifica o elemento. O valor fica sempre entre aspas.</p>`,
    code:`<flor tipo="rosa"/>` },
  { term:"Elemento vs. atributo", cat:"coral",
    body:`<p>Usa <b>elementos</b> para conteúdo principal e estruturas que podem crescer; usa <b>atributos</b> para identificadores e metadados curtos. Não há regra universal — vale a consistência.</p>` },
  { term:"Bem formado vs. válido", cat:"yellow",
    body:`<p><b>Bem formado</b> = respeita a sintaxe (uma raiz, tags fechadas, aninhamento correto, aspas). <b>Válido</b> = além disso, respeita o contrato do domínio definido por um XSD. Um documento pode estar bem formado e, ainda assim, ser inválido.</p>` },
  { term:"Declaração XML", cat:"violet",
    body:`<p>A primeira linha, opcional na 1.0 mas recomendada. Indica versão e codificação. Nunca pode haver nada antes dela.</p>`,
    code:`<?xml version="1.0" encoding="UTF-8"?>` },
  { term:"XSD", cat:"",
    body:`<p>XML Schema Definition — um ficheiro <code>.xsd</code> separado que define as regras do <code>.xml</code>: elementos, atributos, ordem, cardinalidade, tipos de dados e restrições. É em si um documento XML.</p>` },
  { term:"Ligar XSD ao XML", cat:"",
    body:`<p>No XML, aponta-se o esquema com o namespace de instância. Sem namespace próprio usa-se <code>xsi:noNamespaceSchemaLocation</code>.</p>`,
    code:`<rede\n  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"\n  xsi:noNamespaceSchemaLocation="mobilidade.xsd">` },
  { term:"Tipos de dados simples", cat:"coral",
    body:`<p>Dão significado ao texto: <code>xs:string</code>, <code>xs:integer</code>, <code>xs:decimal</code>, <code>xs:boolean</code>, <code>xs:date</code>, <code>xs:dateTime</code>… Decimais usam ponto; datas seguem ISO 8601.</p>` },
  { term:"restriction &amp; facetas", cat:"coral",
    body:`<p>Dentro de um <code>simpleType</code>, a <code>restriction</code> limita um tipo base. As facetas incluem limites de valor, tamanho, padrão e enumeração.</p>`,
    code:`<xs:simpleType name="DisponibilidadeType">\n  <xs:restriction base="xs:nonNegativeInteger">\n    <xs:maxInclusive value="80"/>\n  </xs:restriction>\n</xs:simpleType>` },
  { term:"pattern (regex)", cat:"yellow",
    body:`<p>Restringe o valor a uma expressão regular. Atenção: a regex de XSD não é exatamente a de JavaScript/PCRE.</p>`,
    code:`<xs:pattern value="9[1236][0-9]{7}"/>` },
  { term:"enumeration", cat:"yellow",
    body:`<p>Define um conjunto fechado de valores permitidos.</p>`,
    code:`<xs:restriction base="xs:string">\n  <xs:enumeration value="Normal"/>\n  <xs:enumeration value="Vegetariano"/>\n  <xs:enumeration value="Vegan"/>\n</xs:restriction>` },
  { term:"complexType", cat:"",
    body:`<p>Descreve elementos com filhos e/ou atributos. Dentro define-se a ordem dos filhos (compositor) e os atributos.</p>`,
    code:`<xs:complexType name="EstacaoType">\n  <xs:sequence>\n    <xs:element name="nome" type="xs:string"/>\n  </xs:sequence>\n  <xs:attribute name="id" use="required"/>\n</xs:complexType>` },
  { term:"sequence · choice · all", cat:"violet",
    body:`<p><b>sequence</b>: todos pela ordem declarada (A→B→C). <b>choice</b>: só uma alternativa (A ou B). <b>all</b>: todos, em qualquer ordem, cada um 0 ou 1 vez (XSD 1.0).</p>` },
  { term:"Cardinalidade (minOccurs/maxOccurs)", cat:"violet",
    body:`<p>Quantas vezes um elemento pode aparecer. Por defeito ambos valem 1. <code>maxOccurs="unbounded"</code> remove o limite. Resumo: <code>1..1</code> obrigatório, <code>0..1</code> opcional, <code>0..*</code> lista opcional, <code>1..*</code> lista não vazia.</p>` },
  { term:"Atributos no XSD", cat:"coral",
    body:`<p>Declaram-se como os elementos, com <code>use="required"</code> ou <code>"optional"</code>, e opcionalmente <code>default</code> ou <code>fixed</code>.</p>`,
    code:`<xs:attribute name="lingua" type="xs:string" use="required"/>` },
  { term:"Tipos nomeados &amp; ref", cat:"",
    body:`<p>Para não repetir código, define-se um <code>simpleType</code>/<code>complexType</code> com <code>name</code> e reutiliza-se via <code>type="..."</code>; elementos/atributos globais referenciam-se com <code>ref</code>.</p>`,
    code:`<xs:element name="cliente" type="TipoPessoa"/>\n<xs:element name="fornecedor" type="TipoPessoa"/>` },
  { term:"Namespaces", cat:"yellow",
    body:`<p>Um URI que identifica um vocabulário e evita colisões quando o mesmo nome tem significados diferentes. Os prefixos (<code>doc:</code>, <code>p:</code>) são atalhos locais; o URI não tem de abrir uma página web.</p>` }
];
function buildConcepts(){
  const box = document.getElementById("conceitos-list");
  if(!box) return;
  box.innerHTML = CONCEPTS.map(c=>{
    const code = c.code ? `<pre class="code">${highlightXml(c.code)}</pre>` : "";
    return `<details class="concept ${c.cat||''}">
      <summary><span class="dot"></span>${c.term}<span class="plus">+</span></summary>
      <div class="cbody"><div class="cbody-inner">${c.body}${code}</div></div>
    </details>`;
  }).join("");
}

/* =========================================================================
   QUIZ
   ========================================================================= */
const QUIZ = [
  { q:"Quantos elementos raiz pode ter um documento XML bem formado?",
    opts:["Exatamente um","Um ou mais","Nenhum, é opcional","Depende do XSD"],
    a:0, e:"Um documento XML tem de ter exatamente um elemento raiz que envolve todos os outros." },
  { q:"<code>&lt;Nome&gt;</code> e <code>&lt;nome&gt;</code> são o mesmo elemento?",
    opts:["Sim, o XML ignora maiúsculas","Não, o XML distingue maiúsculas de minúsculas","Só se o XSD disser","Apenas em UTF-8"],
    a:1, e:"XML é case-sensitive: Nome ≠ nome ≠ NOME." },
  { q:"Um documento <b>bem formado</b> está automaticamente <b>válido</b>?",
    opts:["Sim, são a mesma coisa","Não — bem formado é sintaxe; válido exige respeitar um contrato (XSD)","Só se tiver declaração","Só com namespaces"],
    a:1, e:"Bem formado = regras de sintaxe. Válido = cumpre as regras de domínio definidas pelo XSD/Schematron/aplicação." },
  { q:"Para que serve principalmente o XSD?",
    opts:["Dar estilo visual ao XML","Definir o contrato: estrutura, tipos, ordem e restrições, para validar o XML","Comprimir o ficheiro","Converter XML em JSON"],
    a:1, e:"O XSD estabelece as regras de estrutura e conteúdo que um documento XML tem de obedecer." },
  { q:"Que atributo permite um número ilimitado de ocorrências de um elemento?",
    opts:["minOccurs=\"0\"","maxOccurs=\"unbounded\"","use=\"required\"","nillable=\"true\""],
    a:1, e:"maxOccurs=\"unbounded\" remove o limite máximo; por defeito maxOccurs é 1." },
  { q:"Qual compositor obriga a que os filhos apareçam exatamente pela ordem declarada?",
    opts:["xs:all","xs:choice","xs:sequence","xs:group"],
    a:2, e:"sequence impõe a ordem (A → B → C). choice escolhe uma; all permite qualquer ordem." },
  { q:"Com <code>&lt;xs:restriction base=\"xs:nonNegativeInteger\"&gt;</code> e <code>maxInclusive=\"80\"</code>, qual valor é REJEITADO?",
    opts:["0","12","80","81"],
    a:3, e:"81 ultrapassa o máximo (≤ 80). Também seriam rejeitados -1 (negativo) e \"doze\" (não numérico)." },
  { q:"Como se associa um XSD sem namespace a um documento XML?",
    opts:["schemaLocation sozinho","xsi:noNamespaceSchemaLocation","xmlns:xsd","import href"],
    a:1, e:"Usa-se xsi:noNamespaceSchemaLocation (com o namespace de instância xsi) a apontar para o ficheiro .xsd." },
  { q:"Qual faceta restringe um valor a um conjunto fechado de hipóteses?",
    opts:["xs:pattern","xs:enumeration","xs:length","xs:minInclusive"],
    a:1, e:"enumeration define a lista exata de valores aceites (ex.: operacional, manutencao)." },
  { q:"Qual faceta usa expressões regulares?",
    opts:["xs:pattern","xs:enumeration","xs:maxLength","xs:fixed"],
    a:0, e:"pattern aplica uma regex (ex.: 9[1236][0-9]{7} para telemóveis). Atenção: a regex de XSD não é igual à de JavaScript/PCRE." },
  { q:"Para um identificador curto como <code>id=\"PT-VIS-07\"</code>, o mais natural é modelar como…",
    opts:["Elemento","Atributo","Comentário","Namespace"],
    a:1, e:"Identificadores e metadados compactos que qualificam um elemento são bons candidatos a atributos." },
  { q:"<code>minOccurs=\"0\"</code> num elemento significa que ele é…",
    opts:["Obrigatório","Opcional","Fixo","Repetido 10x"],
    a:1, e:"minOccurs=\"0\" torna o elemento opcional (pode estar ausente). Cardinalidade 0..1 ou 0..*." }
];
let quizState = [];
let quizPage = 0;
const QUIZ_PAGE = 3;

function buildQuiz(resetAnswers){
  if(resetAnswers || quizState.length !== QUIZ.length) quizState = QUIZ.map(()=>null);
  quizPage = 0;
  renderQuizPage();
}
function quizPages(){ return Math.ceil(QUIZ.length / QUIZ_PAGE); }
function renderQuizPage(){
  const list = document.getElementById("quizList");
  const start = quizPage * QUIZ_PAGE;
  const slice = QUIZ.slice(start, start + QUIZ_PAGE);
  list.innerHTML = slice.map((item, idx)=>{
    const qi = start + idx;
    const opts = item.opts.map((o,oi)=>`
      <div class="opt" data-q="${qi}" data-o="${oi}">
        <span class="mk"></span><span>${o}</span>
      </div>`).join("");
    return `<div class="q" id="q${qi}">
      <div class="qh"><span class="qn">${String(qi+1).padStart(2,'0')}</span><span class="qt">${item.q}</span></div>
      <div class="opts">${opts}</div>
      <div class="explain" id="e${qi}"></div>
    </div>`;
  }).join("");
  list.querySelectorAll(".opt").forEach(opt=>opt.addEventListener("click", onAnswer));
  slice.forEach((item, idx)=>{ const qi = start + idx; if(quizState[qi]!=null) applyAnswered(qi); });
  renderPager();
  updateScore();
}
function renderPager(){
  const pager = document.getElementById("quizPager");
  const total = quizPages();
  pager.innerHTML =
    `<button class="btn primary" id="prevPg" ${quizPage===0?'disabled':''}>← Anteriores</button>
     <span class="pg">Página ${quizPage+1} de ${total}</span>
     <button class="btn primary" id="nextPg" ${quizPage>=total-1?'disabled':''}>Seguintes →</button>`;
  const prev = document.getElementById("prevPg"), next = document.getElementById("nextPg");
  if(prev) prev.addEventListener("click", ()=>{ if(quizPage>0){ quizPage--; renderQuizPage(); scrollQuizTop(); } });
  if(next) next.addEventListener("click", ()=>{ if(quizPage<total-1){ quizPage++; renderQuizPage(); scrollQuizTop(); } });
}
function scrollQuizTop(){ document.getElementById("quiz").scrollIntoView({behavior:"smooth", block:"start"}); }
function applyAnswered(qi){
  const item = QUIZ[qi];
  const chosen = quizState[qi];
  const container = document.getElementById("q"+qi);
  if(!container) return;
  container.querySelectorAll(".opt").forEach(o=>{
    const idx = +o.dataset.o;
    o.classList.add("disabled");
    if(idx===item.a){ o.classList.add("correct"); o.querySelector(".mk").textContent="✓"; }
    else if(idx===chosen){ o.classList.add("wrong"); o.querySelector(".mk").textContent="✗"; }
  });
  const ex = document.getElementById("e"+qi);
  const right = chosen===item.a;
  ex.innerHTML = `<b>${right?"Certo! ":"Não exatamente. "}</b>${item.e}`;
  ex.classList.add("show");
}
function onAnswer(ev){
  const opt = ev.currentTarget;
  const qi = +opt.dataset.q, oi = +opt.dataset.o;
  if(quizState[qi]!=null) return;
  quizState[qi] = oi;
  applyAnswered(qi);
  updateScore();
}
function updateScore(){
  const answered = quizState.filter(x=>x!=null).length;
  const correct = quizState.reduce((n,v,i)=> n + (v!=null && v===QUIZ[i].a ? 1:0), 0);
  document.getElementById("score").textContent = `${correct} / ${QUIZ.length}`;
  document.getElementById("progBar").style.width = (answered/QUIZ.length*100)+"%";
}

/* =========================================================================
   ANIMAÇÃO ao scroll (revelação suave)
   ========================================================================= */
function initScrollAnim(){
  const reduce = window.matchMedia && window.matchMedia('(prefers-reduced-motion:reduce)').matches;
  if(reduce || !('IntersectionObserver' in window)) return;
  const els = Array.from(document.querySelectorAll('.card, .sec-head, .concept'));
  els.forEach(e=>e.classList.add('anim-in'));
  // leve escalonamento dentro de cada grelha
  document.querySelectorAll('.cards, #conceitos-list').forEach(grid=>{
    Array.from(grid.children).forEach((c,i)=>{
      if(c.classList.contains('anim-in')) c.style.transitionDelay = Math.min(i,6)*55 + 'ms';
    });
  });
  const io = new IntersectionObserver(entries=>{
    entries.forEach(en=>{ if(en.isIntersecting){ en.target.classList.add('in'); io.unobserve(en.target); } });
  }, { threshold:0.1, rootMargin:'0px 0px -40px 0px' });
  els.forEach(e=>io.observe(e));
}

/* =========================================================================
   ABRIR/FECHAR com animação (conceitos e toggles) — fluido e repetível
   Técnica grid-template-rows 0fr↔1fr: anima a altura sem travas nem saltos.
   ========================================================================= */
function animateDetails(d, body){
  const summary = d.querySelector('summary');
  if(!summary || !body) return;
  summary.addEventListener('click', (e)=>{
    e.preventDefault();
    const reduce = window.matchMedia && window.matchMedia('(prefers-reduced-motion:reduce)').matches;
    if(d.dataset.animating === '1') return;

    if(!d.open){
      // ABRIR — altura e fade em conjunto
      d.open = true;             // conteúdo entra no DOM
      d.classList.add('expanded'); // estado final (opacity 1 + rotação do +)
      if(reduce) return;
      d.dataset.animating = '1';
      const h = body.scrollHeight;
      const anim = body.animate(
        [{ height:'0px', opacity:0 }, { height:h+'px', opacity:1 }],
        { duration:300, easing:'cubic-bezier(.4,0,.2,1)' }
      );
      anim.onfinish = anim.oncancel = ()=>{ d.dataset.animating=''; };
    } else {
      // FECHAR — altura e fade em conjunto
      if(reduce){ d.classList.remove('expanded'); d.open=false; return; }
      d.dataset.animating = '1';
      const h = body.scrollHeight;
      d.classList.remove('expanded');
      const anim = body.animate(
        [{ height:h+'px', opacity:1 }, { height:'0px', opacity:0 }],
        { duration:280, easing:'cubic-bezier(.4,0,.2,1)' }
      );
      anim.onfinish = anim.oncancel = ()=>{ d.open=false; d.dataset.animating=''; };
    }
  });
}
function initToggleAnim(){
  document.querySelectorAll('.concept').forEach(d=> animateDetails(d, d.querySelector('.cbody')));
  document.querySelectorAll('.reveal').forEach(d=> animateDetails(d, d.querySelector('.body')));
}

/* =========================================================================
   INIT
   ========================================================================= */
document.addEventListener("DOMContentLoaded", ()=>{
  paintSnippets();
  buildPresets();
  buildConcepts();
  initToggleAnim();
  buildQuiz();
  setupEditor("xmlIn", "xmlHL");
  setupEditor("xsdIn", "xsdHL");
  document.getElementById("runBtn").addEventListener("click", runValidation);
  document.getElementById("clearBtn").addEventListener("click", ()=>{
    document.getElementById("xmlIn").value="";
    document.getElementById("xsdIn").value="";
    refreshEditors();
    document.getElementById("result").innerHTML="";
  });
  document.getElementById("resetQuiz").addEventListener("click", ()=>buildQuiz(true));
  loadPreset(0); // arranca com a rede válida
  initScrollAnim();

  // menu hambúrguer (telemóvel)
  const burger = document.getElementById('burger');
  const navLinks = document.getElementById('navLinks');
  if(burger && navLinks){
    const setOpen = (open)=>{
      navLinks.classList.toggle('open', open);
      burger.setAttribute('aria-expanded', open ? 'true' : 'false');
      burger.setAttribute('aria-label', open ? 'Fechar menu' : 'Abrir menu');
    };
    burger.addEventListener('click', ()=> setOpen(!navLinks.classList.contains('open')));
    navLinks.querySelectorAll('a').forEach(a=> a.addEventListener('click', ()=> setOpen(false)));
    document.addEventListener('click', (e)=>{
      if(navLinks.classList.contains('open') && !navLinks.contains(e.target) && !burger.contains(e.target)) setOpen(false);
    });
  }
});
