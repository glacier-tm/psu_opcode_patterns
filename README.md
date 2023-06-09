# psu opcode patterns (from may 2021) *MIRROR*
made by cyclops. note: make your own handlers idgaf

```cpp
      // just add your opcode priority enum here etc etc
	template <class handlerType>
	struct handlerRegister final { // to register a handler
		static_assert(std::is_function<handlerType>::value || std::is_constructible<std::string, handlerType>::value, "need a string/function handler."); // can't also be a POD type
	private:
		handlerType handler;
	public:
		std::string_view pattern{}; // default initialized
		constexpr auto getHandler() const noexcept {
			return this->handler;
		}
        handlerRegister(std::string_view s, handlerType&& handler) noexcept : pattern{ std::move(s) }, handler(std::forward<handlerType>(handler)) { }; //handler as a rvalue handlertype
        ~handlerRegister() = default;
	}
  
       //string pattern is just instantiated hadnlerRegister<std::string_view>
  	inline std::map<int, stringPattern> opcodeStringPatterns = { //could've made it a std::set but shrug
        	{ARTIMETIC_INSTRUCTION, { R"(\b(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s=\s([L_\d\[\]\s]+(?=([+*\/\-%])))\3\s(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])[\r\n;])", "$1=artimetic_instr_handler($2,\'$3\',$4)" }},
        	{LEN_INSTRUCTION, { R"((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s*?=\s*?#(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])[\r\n;])", "$1=len_handler($2)"}},
        	{CONCAT_INSTRUCTION, { R"(\bfor\s*?L_\d+_forvar\d+\s*?=\s*?L_\d+_\s*?(?:\+\s*?1)?,\s*?(?:L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s*?do\s+(L_\d+_)\s*?=\s*?\1\s\.{2}\s*?L_\d+_\[L_\d+_forvar\d+\][;]?\s+end[;\r\n])", "figure it yourself idiot" }},
        	{GETTABLE_INSTRUCTION, { R"(\b(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\s*?=\s*?(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\[(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\][\r\n;])", "$1=gettable_handler($2,$3)" }},
        	{SETTABLE_INSTRUCTION, { R"(\b(L_\d+_\[L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]\])\[(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\]\s*?=\s*?(L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]))", "figure it yourself idiot" }},
        	{CALL_INSTRUCTION, { R"((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\((?:(L_\d+_\[L_\d+_\s?\+\s?1\])?|(L_\d+_\(L_\d+_,\s*?L_\d+_\s*?\+\s*?1,\s*?(?:L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]))?)\))", "todo" }},
        	{COMPARE_INSTRUCTION, { R"(\bif\s*?\((L_\d+_\[L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]\])\s*?(~=|==|>|<|>=|<=)\s*?(L_\d+_\[L_\d+_\[L_\d+_(?:\[L_\d+_\])?\]\])\)\s*?then\s+L_\d+_\s*?=\s*?L_\d+_\[L_\d+_\][\r\n;]\s*?end)", "figure it yourself idiot" }},
       		{IF_STMT_INSTRUCTION, { R"(\bif\s*?\((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\)\s*?then\s+L_\d+_\s*?=\s*?L_\d+_\[L_\d+_\][;\r\n]\s*?end)", "figure it yourself idiot" }},
        	{NOT_STMT_INSTRUCTION, { R"(\bif\s*?\(not\((L_\d+_\[L_\d+_(?:\[L_\d+_\])?\])\)\)\s*?then\s+L_\d+_\s*?=\s*?L_\d+_\[L_\d+_\][;\r\n]\s*?end)", "figure it yourself idiot" }}
	}
  
```
