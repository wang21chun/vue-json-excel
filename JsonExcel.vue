<template>
<button :class="className" @click="generate">
    <slot>
        {{button_text}}
    </slot>
</button>
</template>

<script>
export default {
    data: function() {
        return {};
    },
    props: {
        'type': {
            type: String,
            default: "xlsx"
        },
        'button_text': {
            type: String,
            default: "Download Excel"
        },
        'data': {
            type: Array,
            required: true
        },
        'fields': {
            type: Array,
            required: true
        },
        'name': {
            type: String,
            default: "defalut"
        }
    },
    created: function() {},
    computed: {

    },
    methods: {
        generate() {
            let data = [].concat(this.data);
            data.unshift(this.fields);
            return this.exportXLSX(data);
        },
        exportXLSX: function(data) {
            function Workbook() {
                if (!(this instanceof Workbook)) return new Workbook();
                this.SheetNames = [];
                this.Sheets = {};
            }

            let ws_name = this.name || "Sheet";
            const wb = new Workbook(),
                ws = this.sheet_from(data);
            wb.SheetNames.push(ws_name);
            wb.Sheets[ws_name] = ws;

            const wbout = XLSX.write(wb, {
                bookType: self.type,
                bookSST: true,
                type: 'binary'
            });
            this.download(wbout);
        },
        sheet_from(data) {
            const ws = {};
            const range = {
                s: {
                    c: 10000000,
                    r: 10000000
                },
                e: {
                    c: 0,
                    r: 0
                }
            };

            for (let R = 0; R != data.length; ++R) {
                for (let C = 0; C != data[R].length; ++C) {
                    if (range.s.r > R) range.s.r = R;
                    if (range.s.c > C) range.s.c = C;
                    if (range.e.r < R) range.e.r = R;
                    if (range.e.c < C) range.e.c = C;
                    const cell = {
                        v: data[R][C]
                    };
                    if (cell.v == null) continue;
                    const cell_ref = XLSX.utils.encode_cell({
                        c: C,
                        r: R
                    });

                    if (typeof cell.v === 'number') cell.t = 'n';
                    else if (typeof cell.v === 'boolean') cell.t = 'b';
                    else if (cell.v instanceof Date) {
                        cell.t = 'n';
                        cell.z = XLSX.SSF._table[14];
                        cell.v = this.datenum(cell.v);
                    } else cell.t = 's';

                    ws[cell_ref] = cell;
                }
            }
            if (range.s.c < 10000000) ws['!ref'] = XLSX.utils.encode_range(range);
            return ws;
        },
        datenum(v, date1904) {
            if (date1904) v += 1462;
            const epoch = Date.parse(v);
            return (epoch - new Date(Date.UTC(1899, 11, 30))) / (24 * 60 * 60 * 1000);
        },
        s2ab(s) {
            const buf = new ArrayBuffer(s.length);
            const view = new Uint8Array(buf);
            for (let i = 0; i != s.length; ++i) view[i] = s.charCodeAt(i) & 0xFF;
            return buf;
        },

        download: function(wbout) {
            const buff = this.s2ab(wbout);
            const blob = new Blob([buff], {
                type: "application/octet-stream"
            });
            saveAs(blob, `${this.name}.${this.type}`);
        } //end download
    }
}
</script>
