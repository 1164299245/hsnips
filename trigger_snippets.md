//t：文本模式。仅在数学之外运行此代码片段	
//m：数学模式。仅在数学运算中运行此代码片段。两者的M简写n
//M：块数学模式。仅在$$ ... $$块内运行此代码片段
//n：内联数学模式。仅在$ ... $块内运行此代码段
//A：自动。输入触发器后立即展开此代码片段。如果省略，Tab则必须按下该键才能展开代码片段, 可选 tab 或 space
//r: Regex。trigger将被视为正则表达式
//v：视觉。仅在选定内容上运行此代码片段。触发器应为单个字符
//w：单词边界。仅当触发器位于单词分隔符（例如 、 或 ）之前（和之后）时，才运行此.代码,段-。		感觉这个是最绕的
//c：代码模式。仅在``` ... ```块内运行此代码段
[
    // Math mode
	//{trigger: "mk", replacement: "$$0$", options: "tA"},
	//{trigger: "dm", replacement: "$$\n$0\n$$", options: "tAw"},
	{trigger: "beg", replacement: "\\begin{$0}\n$1\n\\end{$0}", options: "mA"},


    // Greek letters	, description:""
	//希腊字母, 函数等自动加反斜杠
	{trigger: "([^\\\\])(${GREEK}|${SYMBOL}|${MORE_SYMBOLS})", replacement: "[[0]]\\[[1]]", options: "rmA", description: "Add backslash before Greek letters"},	//	组 ([^\\\\]) 匹配一个不是反斜杠的字符，js和Regex各转义一次，一个变四个。
	{trigger: /([^\\])(arcsin|sin|arccos|cos|arctan|tan|csc|sec|cot)/, replacement: "[[0]]\\[[1]]", options: "rmA", description: "Add backslash before trig funcs",priority :1}, 	//有限度1保证 \sin 不会变成 s\in, 正则贪婪保证 arcsin 优先于 sin 

	{trigger: "alf", replacement: "\\alpha", options: "mA", description:"α"},
	{trigger: "bta", replacement: "\\beta", options: "mA", description:"β"},
	{trigger: "gama", replacement: "\\gamma", options: "mA", description:"γ"},
		{trigger: "\\gamma~", replacement: "\\Gamma", options: "mA", description:"γ to Γ"},
	{trigger: "dlt", replacement: "\\delta", options: "mA", description:"δ"},
		{trigger: "\\delta~", replacement: "\\Delta", options: "mA", description:"δ to Δ"},
	{trigger: "epsl", replacement: "\\varepsilon", options: "mA", description:"ε"},
	//{trigger: ":e", replacement: "\\epsilon", options: "mA"},
	{trigger: "zta", replacement: "\\zeta", options: "mA", description:"ζ"},
	{trigger: "cta", replacement: "\\theta", options: "mA", description:"θ"},
	{trigger: "lml", replacement: "\\lambda", options: "mA"},
		{trigger: "\\lambda~", replacement: "\\Lambda", options: "mA"},
	{trigger: "sgm", replacement: "\\sigma", options: "mA", description:"σ"},
		{trigger: "\\sigma~", replacement: "\\Sigma", options: "mA", description:"Σ"},
	{trigger: "omg", replacement: "\\omega", options: "mA", description:"ω"},
		{trigger: "\\omega~", replacement: "\\Omega", options: "mA", description:"Ω"},
	{trigger: "miu", replacement: "\\mu", options: "mA", description:"μ"},
	{trigger: "niu", replacement: "\\nu", options: "mA", description:"ν"},
	{trigger: "ksi", replacement: "\\xi", options: "mA", description:"ξ"},
	{trigger: "rou", replacement: "\\rho", options: "mA", description:"ρ"},
	{trigger: "tao", replacement: "\\tau", options: "mA", description:"τ"},
	{trigger: "fai", replacement: "\\varphi", options: "mA", description:"φ"},
		{trigger: "\\varphi~", replacement: "\\Phi", options: "mA", description:"Φ"},
		{trigger: "\\psi~", replacement: "\\Psi", options: "mA", description:"Ψ"},
	{trigger: "kai", replacement: "\\chi", options: "mA", description:"χ"},
	//{trigger: "@u", replacement: "\\upsilon", options: "mA"},
	//{trigger: "@U", replacement: "\\Upsilon", options: "mA"},
	//{trigger: "@T", replacement: "\\Theta", options: "mA", description:"capital Θ"},
	//{trigger: ":t", replacement: "\\vartheta", options: "mA"},
	//{trigger: "@i", replacement: "\\iota", options: "mA"},
	//{trigger: "@k", replacement: "\\kappa", options: "mA"},



	// More auto letter subscript 下标, 如果不希望触发下标就加个空格, 例如 sin1 会变成 sin_1 而 sin 1 不会
	//逻辑是，a1这种会直接变，下标是字母 n或者n+1的要按一下` ，下标变上标要按 ~ ，很合理吧，`不用按shift，~要按，正好对应上和下，我写了很久才弄出这套折中的办法。
    {trigger: /([A-Za-z])(\d)/, replacement: "[[0]]_[[1]]", options: "rmA", description:"a + 1 → a_1"},	//当然\alpha的末尾字母a也会触发
	{trigger: /([A-Za-z])_(\d)~/, replacement: "[[0]]^[[1]]", options: "rmA", description:"a_1 → a^1"},

    {trigger: /([A-Za-z]\^|_)(\d\d)/, replacement: "[[0]]{[[1]]}", options: "rmA", description:"a_1 + 2 → a_{12}"},
	{trigger: /([A-Za-z])_(\{\d\d\})~/, replacement: "[[0]]^[[1]]", options: "rmA", description:"a_{12} → a^{12}"},

	{trigger: /([A-Za-z])([mnij]([+-]\d)?)`/, replacement: "[[0]]_{[[1]]}", options: "rmA", description:"an+1 → a_{n+1}"},
	{trigger: /([A-Za-z])_(\{[mnij]([+-]\d)?\})~/, replacement: "[[0]]^[[1]]", options: "rmA", description:"a_{n+1} → a^{n+1}"},






    // Basic operations
	{trigger: "_", replacement: "_{$0}$1", options: "mA"},
	{trigger: "sts", replacement: "_\\text{$0}", options: "mA"},
	{trigger: "sq", replacement: "\\sqrt{ $0 }$1", options: "mA"},
	{trigger: "//", replacement: "\\frac{$0}{$1}$2", options: "mA"},
	{trigger: "ee", replacement: "e^{ $0 }$1", options: "mA"},
    {trigger: "invs", replacement: "^{-1}", options: "mA"},

    //{trigger: /([^\\])(exp|log|ln)/, replacement: "[[0]]\\[[1]]", options: "rmA"},
    {trigger: "conj", replacement: "^{*}", options: "mA"},
    {trigger: "Re", replacement: "\\mathrm{Re}", options: "mA"},
	{trigger: "Im", replacement: "\\mathrm{Im}", options: "mA"},
    {trigger: "bf", replacement: "\\mathbf{$0}", options: "mA"},
	{trigger: "rm", replacement: "\\mathrm{$0}$1", options: "mA"},

    // Linear algebra
    {trigger: /([^\\])(det)/, replacement: "[[0]]\\[[1]]", options: "rmA"},
    {trigger: "trace", replacement: "\\mathrm{Tr}", options: "mA"},


	// 特殊字体
	{trigger: /([A-Z])rm/, replacement: "\\mathrm{[[0]]}", options: "rmA"},	//	芝士 mathrm
	{trigger: /([a-z])rm/, replacement: (match) => {return "\\mathrm{" + match[1].toUpperCase() + "}"}, options: "rmA"},

	{trigger: /([A-Z])bf/, replacement: "\\mathbf{[[0]]}", options: "rmA"},	//	芝士 mathbf
	{trigger: /([a-z])bf/, replacement: (match) => {return "\\mathbf{" + match[1].toUpperCase() + "}"}, options: "rmA"},	

	{trigger: /([A-Z])bb/, replacement: "\\mathbb{[[0]]}", options: "rmA"},	//	芝士 mathbb
	{trigger: /([a-z])bb/, replacement: (match) => {return "\\mathbb{" + match[1].toUpperCase() + "}"}, options: "rmA"},	

	{trigger: /([A-Z])cal/, replacement: "\\mathcal{[[0]]}", options: "rmA"},	//	芝士 mathcal
	{trigger: /([a-z])cal/, replacement: (match) => {return "\\mathcal{" + match[1].toUpperCase() + "}"}, options: "rmA"},	

	{trigger: /([A-Z])frak/, replacement: "\\mathfrak{[[0]]}", options: "rmA"},	//	芝士 mathfrak
	{trigger: /([a-z])frak/, replacement: (match) => {return "\\mathfrak{" + match[1].toUpperCase() + "}"}, options: "rmA"},

    // More operations 头顶操作：一弯弯，尖尖，一bar
	
	{trigger: "([a-zA-Z])hat", replacement: "\\hat{[[0]]}", options: "rmA"},
		{trigger: /\\hat{([A-Za-z])}(\d)/, replacement: "\\hat{[[0]]}_{[[1]]}", options: "rmA"},
    {trigger: "([a-zA-Z])bar", replacement: "\\overline{[[0]]}", options: "rmA"},
	{trigger: "([a-zA-Z])dot", replacement: "\\dot{[[0]]}", options: "rmA", priority: -1},
	{trigger: "([a-zA-Z])ddot", replacement: "\\ddot{[[0]]}", options: "rmA", priority: 1},
	{trigger: "([a-zA-Z])tilde", replacement: "\\tilde{[[0]]}", options: "rmA"},
	{trigger: "([a-zA-Z])und", replacement: "\\underline{[[0]]}", options: "rmA"},
	{trigger: "([a-zA-Z])vec", replacement: "\\vec{[[0]]}", options: "rmA"},
		{trigger: /\\vec{([A-Za-z])}(\d)/, replacement: "\\vec{[[0]]}_{[[1]]}", options: "rmA"},
    {trigger: "([a-zA-Z]),\\.", replacement: "\\mathbf{[[0]]}", options: "rmA"},
	{trigger: "([a-zA-Z])\\.,", replacement: "\\mathbf{[[0]]}", options: "rmA"},
	{trigger: "\\\\(${GREEK}),\\.", replacement: "\\boldsymbol{\\[[0]]}", options: "rmA"},
	{trigger: "\\\\(${GREEK})\\.,", replacement: "\\boldsymbol{\\[[0]]}", options: "rmA"},

	{trigger: "hat", replacement: "\\hat{$0}$1", options: "mA"},
    {trigger: "bar", replacement: "\\bar{$0}$1", options: "mA"},
	{trigger: "dot", replacement: "\\dot{$0}$1", options: "mA", priority: -1},
	{trigger: "ddot", replacement: "\\ddot{$0}$1", options: "mA"},
	{trigger: "cdot", replacement: "\\cdot", options: "mA"},
	{trigger: "tilde", replacement: "\\tilde{$0}$1", options: "mA"},
	{trigger: "und", replacement: "\\underline{$0}$1", options: "mA"},
	{trigger: "vec", replacement: "\\vec{$0}$1", options: "mA"},


    // Symbols
    {trigger: "eqeq", replacement: "\\xlongequal{$0}{}", options: "mA"},	
    {trigger: "ker", replacement: "\\mathrm{Ker}", options: "mA"},
    {trigger: "ima", replacement: "\\mathrm{Im}", options: "mA"},	
    {trigger: "wuq", replacement: "\\infty", options: "mA"},
    {trigger: "chac", replacement: "\\times", options: "mA"},
    {trigger: "dianc", replacement: "\\cdot", options: "mA"},
    {trigger: "...", replacement: "\\cdots", options: "mA"},


    // Handle spaces and backslashes

    // Snippet variables can be used as shortcuts when writing snippets.
    // For example, ${GREEK} below is shorthand for "alpha|beta|gamma|Gamma|delta|..."
    // You can edit snippet variables under the Advanced snippet settings section.


    // Insert space after Greek letters and symbols
	{trigger: "\\\\(${GREEK}|${SYMBOL}|${MORE_SYMBOLS})([A-Za-z])", replacement: "\\[[0]] [[1]]", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) sr", replacement: "\\[[0]]^{2}", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) cb", replacement: "\\[[0]]^{3}", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) rd", replacement: "\\[[0]]^{$0}$1", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) hat", replacement: "\\hat{\\[[0]]}", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) dot", replacement: "\\dot{\\[[0]]}", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) bar", replacement: "\\bar{\\[[0]]}", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) vec", replacement: "\\vec{\\[[0]]}", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) tilde", replacement: "\\tilde{\\[[0]]}", options: "rmA"},
	{trigger: "\\\\(${GREEK}|${SYMBOL}) und", replacement: "\\underline{\\[[0]]}", options: "rmA"},


    // Derivatives and integrals
    {trigger: "par", replacement: "\\frac{ \\partial ${0:y} }{ \\partial ${1:x} } $2", options: "m"},
    {trigger: /pa([A-Za-z])([A-Za-z])/, replacement: "\\frac{ \\partial [[0]] }{ \\partial [[1]] } ", options: "rm"},
    {trigger: "ddt", replacement: "\\frac{d}{dt} ", options: "mA"},

    {trigger: /([^\\])int/, replacement: "[[0]]\\int", options: "mA", priority: -1},
    {trigger: "\\int", replacement: "\\int $0 \\, d${1:x} $2", options: "m"},
    {trigger: "dint", replacement: "\\int_{${0:0}}^{${1:1}} $2 \\, d${3:x} $4", options: "mA"},
    {trigger: "oint", replacement: "\\oint", options: "mA"},
	{trigger: "iint", replacement: "\\iint", options: "mA"},
    {trigger: "iiint", replacement: "\\iiint", options: "mA"},
    {trigger: "oinf", replacement: "\\int_{0}^{\\infty} $0 \\, d${1:x} $2", options: "mA"},
	{trigger: "infi", replacement: "\\int_{-\\infty}^{\\infty} $0 \\, d${1:x} $2", options: "mA"},


    // Trigonometry

    {trigger: /\\(arcsin|sin|arccos|cos|arctan|tan|csc|sec|cot)([A-Za-gi-z])/,
     replacement: "\\[[0]] [[1]]", options: "rmA",
     description: "Add space after trig funcs. Skips letter h to allow sinh, cosh, etc."},

    {trigger: /\\(sinh|cosh|tanh|coth)([A-Za-z])/,
     replacement: "\\[[0]] [[1]]", options: "rmA",
     description: "Add space after hyperbolic trig funcs"},





    // Physics
	{trigger: "kbt", replacement: "k_{B}T", options: "mA"},
	{trigger: "msun", replacement: "M_{\\odot}", options: "mA"},

    // Quantum mechanics
    {trigger: "dag", replacement: "^{\\dagger}", options: "mA"},
	//{trigger: "o+", replacement: "\\oplus ", options: "mA"},
	{trigger: "ox", replacement: "\\otimes ", options: "mA"},
    {trigger: "bra", replacement: "\\bra{$0} $1", options: "mA"},
	{trigger: "ket", replacement: "\\ket{$0} $1", options: "mA"},
	{trigger: "brk", replacement: "\\braket{ $0 | $1 } $2", options: "mA"},
    {trigger: "outer", replacement: "\\ket{${0:\\psi}} \\bra{${0:\\psi}} $1", options: "mA"},

    // Chemistry
	//{trigger: "pu", replacement: "\\pu{ $0 }", options: "mA"},
	//{trigger: "cee", replacement: "\\ce{ $0 }", options: "mA"},
	//{trigger: "he4", replacement: "{}^{4}_{2}He ", options: "mA"},
	//{trigger: "he3", replacement: "{}^{3}_{2}He ", options: "mA"},
	//{trigger: "iso", replacement: "{}^{${0:4}}_{${1:2}}${2:He}", options: "mA"},


    // Environments
	//{trigger: "pmat", replacement: "\\begin{pmatrix}\n$0\n\\end{pmatrix}", options: "MA"},
	//{trigger: "bmat", replacement: "\\begin{bmatrix}\n$0\n\\end{bmatrix}", options: "MA"},
	//{trigger: "Bmat", replacement: "\\begin{Bmatrix}\n$0\n\\end{Bmatrix}", options: "MA"},
	//{trigger: "vmat", replacement: "\\begin{vmatrix}\n$0\n\\end{vmatrix}", options: "MA"},
	//{trigger: "Vmat", replacement: "\\begin{Vmatrix}\n$0\n\\end{Vmatrix}", options: "MA"},
	//{trigger: "matrix", replacement: "\\begin{matrix}\n$0\n\\end{matrix}", options: "MA"},

	//{trigger: "pmat", replacement: "\\begin{pmatrix}$0\\end{pmatrix}", options: "nA"},
	//{trigger: "bmat", replacement: "\\begin{bmatrix}$0\\end{bmatrix}", options: "nA"},
	//{trigger: "Bmat", replacement: "\\begin{Bmatrix}$0\\end{Bmatrix}", options: "nA"},
	//{trigger: "vmat", replacement: "\\begin{vmatrix}$0\\end{vmatrix}", options: "nA"},
	//{trigger: "Vmat", replacement: "\\begin{Vmatrix}$0\\end{Vmatrix}", options: "nA"},
	//{trigger: "matrix", replacement: "\\begin{matrix}$0\\end{matrix}", options: "nA"},

	//{trigger: "cases", replacement: "\\begin{cases}\n$0\n\\end{cases}", options: "mA"},
	//{trigger: "align", replacement: "\\begin{align}\n$0\n\\end{align}", options: "mA"},
	//{trigger: "array", replacement: "\\begin{array}\n$0\n\\end{array}", options: "mA"},


    // Brackets
	{trigger: "avg", replacement: "\\langle $0 \\rangle $1", options: "mA"},
	{trigger: "norm", replacement: "\\lvert $0 \\rvert $1", options: "mA", priority: 1},
	{trigger: "Norm", replacement: "\\lVert $0 \\rVert $1", options: "mA", priority: 1},
	{trigger: "ceil", replacement: "\\lceil $0 \\rceil $1", options: "mA"},
	{trigger: "floor", replacement: "\\lfloor $0 \\rfloor $1", options: "mA"},
	{trigger: "mod", replacement: "|$0|$1", options: "mA"},
	{trigger: "(", replacement: "($0)", options: "mA"},
	{trigger: "{", replacement: "{$0}", options: "mA"},
	{trigger: "[", replacement: "[$0]", options: "mA"},
	{trigger: "()(", replacement: "\\left( $0 \\right)", options: "mA"},
	{trigger: "{}{", replacement: "\\left\\{ $0 \\right\\}", options: "mA"},
	{trigger: "[][", replacement: "\\left[ $0 \\right]", options: "mA"},
	{trigger: "lr|", replacement: "\\left| $0 \\right| $1", options: "mA"},
	{trigger: "<><", replacement: "\\left< $0 \\right>", options: "mA"},

    // Misc

    // Automatically convert standalone letters in text to math (except a, A, I).
    // (Un-comment to enable)
    // {trigger: /([^'])\b([B-HJ-Zb-z])\b([\n\s.,?!:'])/, replacement: "[[0]]$[[1]]$[[2]]", options: "tA"},

    // Automatically convert Greek letters in text to math.
    // {trigger: "(${GREEK})([\\n\\s.,?!:'])", replacement: "$\\[[0]]$[[1]]", options: "rtAw"},

    // Automatically convert text of the form "x=2" and "x=n+1" to math.
    // {trigger: /([A-Za-z]=\d+)([\n\s.,?!:'])/, replacement: "$[[0]]$[[1]]", options: "rtAw"},
    // {trigger: /([A-Za-z]=[A-Za-z][+-]\d+)([\n\s.,?!:'])/, replacement: "$[[0]]$[[1]]", options: "tAw"},


    // Snippet replacements can have placeholders.
	{trigger: "tayl", replacement: "${0:f}(${1:x} + ${2:h}) = ${0:f}(${1:x}) + ${0:f}'(${1:x})${2:h} + ${0:f}''(${1:x}) \\frac{${2:h}^{2}}{2!} + \\dots$3", options: "mA", description: "Taylor expansion"},

//Visual, trigger should be the single character==========================================================================
	{trigger: "(", replacement: "(${VISUAL})", options: "mA"},
	{trigger: "[", replacement: "[${VISUAL}]", options: "mA"},
	{trigger: "{", replacement: "{${VISUAL}}", options: "mA"},
	{trigger: "s", replacement: "\\underbracket{ ${VISUAL} }_{ $0 }", options: "mA", description: "下方括号"},
	{trigger: "w", replacement: "\\overbracket{ ${VISUAL} }^{ $0 }", options: "mA", description: "上方括号"},
	{trigger: "B", replacement: "\\underset{ $0 }{ ${VISUAL} }", options: "mA"},
	{trigger: "C", replacement: "\\cancel{ ${VISUAL} }", options: "mA"},
	{trigger: "K", replacement: "\\cancelto{ $0 }{ ${VISUAL} }", options: "mA"},
	{trigger: "S", replacement: "\\sqrt{ ${VISUAL} }", options: "mA"},


//左大括号
	{trigger: "cases", replacement: "\\left\\\{ \\begin{aligned}\n$0\n\\end{aligned} \\right.", options: "mA", description:"左大括号"},
	{trigger: "==", replacement: "&=", options: "mA", description: "aligned内对齐占位符"},

//中文数学切换
	{trigger: /([A-Za-z])vv/, replacement: "$[[0]]$$0", options: "rtwA", description:"我Kvv →我 $K$ "},
	{trigger: /([一-龥]) vv/, replacement: "[[0]]$$0$", options: "rtA", description:"汉字后面按vv直接进入mathmode"},
// Text environment
    {trigger: "txt", replacement: "\\text{ $0 }$1", options: "mA", description:"注意占位符$0两边有空格以在中文与数字中间空格"},
    {trigger: "\"", replacement: "\\text{ $0 }$1", options: "mA", description:"按一个双引号直接进入text"},



//大型函数=====================================================================================================================
//组循环
	{trigger: /(\\cdots|\\ddots|\\vdots)`/, replacement: (match) => {
		var arr = ["\\cdots","\\ddots","\\vdots"];	//	变量 array 数组
		var str = match[1];							//	变量 string 匹配正则
		var index = arr.indexOf(str) + 1;			//	变量,指向下一个
		if (index < arr.length) {
			return arr[index]; // 返回数组中的下一个元素
		}
		else {
			return arr[0];    // 如果已经是最后一个，则回到第一个元素
		}
	}
	, options: "mA", description: "组循环"},
	
    // Snippet replacements can also be JavaScript functions.
    // See the documentation for more information.
	{trigger: /iden(\d)/, replacement: (match) => {
		const n = match[1];

		let arr = [];
		for (let j = 0; j < n; j++) {
			arr[j] = [];
			for (let i = 0; i < n; i++) {
				arr[j][i] = (i === j) ? 1 : 0;
			}
		}

		let output = arr.map(el => el.join(" & ")).join(" \\\\\n");
		output = `\\begin{pmatrix}\n${output}\n\\end{pmatrix}`;
		return output;
	}, options: "mA", description: "N x N identity matrix"},

	//小试牛刀，把hsnip里写的js代码在这试一下
	{trigger: /mx_([1-9])([1-9])/, replacement: (match) => {
		const nrow = match[1];	//匹配row
		const ncol = match[2];	//匹配column

		//let IndexOfNormalMatrix = (nrow + ncol).toString();	//根据矩阵尺寸而定的最后一个占位符，用来修饰matrix的样式例如pmat,bmat
		//我突然发现不用这个变量, 只要让order加到最后不就行了吗.
		let rv = "\\begin{${0:p}matrix}" + "\n" ;	//输出首行的beg{?matrix}, ?是最后一个占位符位置
		let order = 1;	//占位符指标, 不同于hsnip的$0结束, 这个语言从$0开始, 没有结束标识
		for (var i = 1; i < nrow; i++){     //除了最后一行
			for (var j = 1; j < ncol; j++){     //除了最后一列
				rv += "$" + (order).toString() + " & ";
				order ++;
			}
			rv += "$" + (order).toString() + "\\\\" + "\n";   //(除去最后一行的)最后一列
			order ++;
		}
		for (var k = 1; k < ncol; k++){     //最后一行
			rv += "$" + (order).toString() + " & ";
			order ++;
		}
		rv += "$" + (order).toString() + "\n";
		order ++;
		rv += "\\end{${0:p}matrix}$" + (order).toString();	//插入一个紧挨$$前的占位符用来离开环境
		return rv;
	}, options: "mA", priority: 2, description: "N x N identity matrix"},//	priority 2 保证不会触发成 mx_{23} 这种下标形式
]
		