# Day 3: Rucksack Reorganization

Codepen: https://codepen.io/mamunoz-dev/pen/GRBvzqm?editors=0012

```js
const priorities = createPriorities();

function createPriorities() {
  const createAlphabetPriorities = (alpha) => alpha.map((x) => String.fromCharCode(x));

  const alphabetLowercase = Array.from(Array(26)).map((e, i) => i + 97);
  const alphabetUppercase = Array.from(Array(26)).map((e, i) => i + 65);

  return createAlphabetPriorities(alphabetLowercase).concat(createAlphabetPriorities(alphabetUppercase));
}

function intersection(arrays) {
  return arrays[0].filter(elem => arrays.every(array => array.includes(elem)));
}

function sliceIntoChunks(arr, chunkSize) {
  const res = [];
  for (let i = 0; i < arr.length; i += chunkSize) {
    const chunk = arr.slice(i, i + chunkSize);
    res.push(chunk);
  }
  return res;
}

function getPriority(sackChunk) {
  const commonItem = intersection(sackChunk)[0];
  return priorities.indexOf(commonItem) + 1;
}

function part1Callback(acc, sack) {
  const firstCompartment = sack.substring(0, sack.length/2).split('');
  const secondCompartment = sack.substring(sack.length/2).split('');

  return acc + getPriority([firstCompartment, secondCompartment]);
}

function part2Callback(acc, sackChunk) {
  const splittedSackChunk = sackChunk.map(sack => sack.split(''));

  return acc + getPriority(splittedSackChunk);
}

function part1(input) {
  return input.reduce((acc, sack) => part1Callback(acc, sack), 0);
}

function part2(input) {
  const sackChunks = sliceIntoChunks(input, 3);
  return sackChunks.reduce((acc, sackChunk) => part2Callback(acc, sackChunk), 0);
}

const sample = ["vJrwpWtwJgWrhcsFMMfFFhFp", "jqHRNqRjqzjGDLGLrsFMfFZSrLrFZsSL", "PmmdzqPrVvPwwTWBwg", "wMqvLMZHhHMvwLHjbvcjnnSBnvTQFn", "ttgJtRGJQctTZtZT", "CrZsJsPPZsGzwwsLwLmpwMDw"];
const input = ["DsPhSBQQQhqmBDhPDsFwjwsLjlRjlttvjvvtRb", "rNJMNNbrHrtjHLHjvwtg", "fNbNzZdrZnMnMPnQShFPDmnqFm", "QWVCFfQffgQCVZzVVpHsHJBqtpspJFRHqq", "mwDbmnnGNlNcwNDDNRbnNDlJTpBJBtJGtPTLsBGqTqqsqp", "MlSdnScRnnmmDjSdNSdCzvggWzrgzjvfvrgVzW", "gsMljbrjlZlWcWMJrWwTwbmwQbmmLDQQLhwL", "CdgpzdgpgnfThHfFRwhfRf", "SptgpSpnCNpVSGNPvPGSddcMWjMrjqBsJcWqMcBWcVlZ", "JcJLQQFWhQJPJpWcwjHvMQvnnlMvzBHd", "tCtGZrmVRmVGTVTtCfRTCHHNNvdNzmdMvMlNzvwdvw", "CTGGRftfSGtGTGDLbFchSgSWWWcM", "QcMFQrvrQbvtczbVbjbMzZzRpqmDDmqqnNzCDCDC", "SHHfPJssGLPSdHThLhHdRmqNmNssnNmNCNnpjmsn", "LhLJfTdLJwfgPTdfGccrtjcMrccwvcrrFr", "jFLLLqDGjbtqLCChpMMhMBvpwMTmffpZ", "ZnJHRncHHgnrsrZffTdMdMBfmMvfvR", "NWWPnZrVHrZPCDDQtzDCPLCq", "jpFjvBZhDFHZdwcmslcslBLLNl", "dVtTVVCzzfrrMPNLLcnVcPLRns", "CrzWzTqdWtGCzJtbJCrMjjDFHZQjZSpvFGFgHhHp", "JjJqMctnhtDZDQtf", "TrFFlrrCCHPwHwlPHFPzDhsffQQDsVfWwVJQJB", "HHHGdFlHldTpCCFFlLHdCRJccMnqvqMgnNjjMjjvLMLcSv", "cMfFcMFcrqgJLFZdrTDdthPGsGmtGs", "WwjNwnjjBQzVVQHwRDnmtPGhPPsPnnTGld", "bWHBjWVzpbRzbmScqgZMFcqf", "RJjPTBJhTNNjfPhRBdqtlgdbGldwtCPgdb", "ZmrHHmzpvSvvpzvmvDVCGlWVwCWtGtWCddggqf", "QQpzFrHHQnzHvfTcNshcLRNFJhcR", "QrPQDrppBQmCmFQm", "TzqzsLfmsfSTfqzVLftNdJJNJGCwwGdgCwSGNC", "TVHTfzWsfftsZstnbvrbWbrbppPbrmmP", "BQrfqrLtJnttqqtQBJDDtBnDzRgldhVVpJlgzpzhpzsgslhV", "TTNcPZCvZjmPFZjvPHLlWdNLghVhzzlllpVd", "jcFPbTcZTFcmcjMjjFjbLwbnDBtfqbtBwfwSBnrbGw", "SZJNJtrNzjjNCzlBBmqmQDBBmDBBjB", "PGPbGwhLsnvwnVbGPVMZsDsTBBgcZgBBDBfDBf", "pnnhhvphGVpvRPnJlSFRNNJHZHSdSR", "hbmDDmwnnVGbhmjNrrWwLNLsWBrw", "dMrgvcQqdjlBLlfW", "HzzPSrcHqFHQgzpPcMpQqrtmbbbRhHHVGnZDnVHhVbVZ", "RwtvdPRvSlTQmHHBQBRL", "FjVSjrsFVnFQnTHmnT", "VCVVMrWWjVGgbcNVGCCVdlvfdqfvwbSzwqfwZzwv", "fmPDwJPDFRmRgPdwwwDNwgwPzhSQzVSzVTQdzHZzHhzQMQzQ", "sWtGCWtpcqqpNnQjjzhVTTVQczhj", "tWBGntsCNlqrWswDRfFrbFrPDbPf", "dmzmjcbQjjQztFNqsqBcMJqNPF", "nlTWnClWwQDTVnTrsMsCRBPZRJRJqRJq", "wTrhwpVVvgvlDpTvVWVDdLfzgbLtbzSQLSzzjzLz", "RWZdHvRdBRGbbvCjJnbn", "pqqpSwzpSSbCwPwjlwjl", "gDzzqSVzqrThpDBtRtCZdQRQLrQQ", "GRqTGqtmTVdGHHVVNNlhPlMqbNqNDbqW", "LzQSdFnfznfwBcLcnFppBBDDMPMPhDbhlwbWPlbWMjrD", "QznFnLZFvVvHRgsddG", "BBHBfBHFdSltmWJvqtNtHq", "TQDrrVMzVDnWnNZDJc", "gWzprTCrCMQzGGjjhwFwdffjfl", "LLLSSSzBBlBLsszncCBZSPSMMMpdWTdrmmnfVGVmMfmrmm", "DghttvwhHRvjJthNJwhVtDHGpddMWdMmrMmfGdppjWTfpq", "gbJRthQDDRvwtDhQhDFPcVcbPczLzPBScBBF", "sZsZpTtLCsbspZtTwrCwrwtDmNNdJmmqSJfqmfNGNmHqGdqb", "nQvjTcjQglFVlllMFMVFqWfNqnNHHHdfSfHqfSJq", "hFvvgFcFVzphpTrrwZDp", "qJqfhsBpfSpchpqcrqwCDvvCDQndmpwDtnRQ", "jWHPZWWjZsCmCWRDmQ", "PPzMGHlzMNsPLPlZsllgsNrhNVBqhFVBbBSqFFBFrV", "RnRsFFdSzmgwvQsqwc", "lbgHGMBHlWWWlfWGGBtGwqDpDwmcpMrrqrQDZrMZ", "GGJLBVJbVGjGtHVfJtWGHBLLPdzdCghnRnRnSShgNnhnddhj", "VgzLFjjwhhSwFhVZgRhRgHHCCvdZdrqqCTvBCrqvvr", "ncnNbGMcPpvHFrssdBHM", "cnctcNNbQGWFJctftgVmzShzzwwVwgmwfR", "pFWmSSFGQlvTbwWTwH", "jdBgNhRgMftNBhPbhHnZHlTTZcwZ", "lfBNRjCtCfMjsBfCjgfNBRMppGpJqsJQGrrmmJDpGmDDFG", "GznngnhzccVdgjbbVjVjVbVLwwQJmQMrLTZhJmZLQTJWmm", "pDBSslPCFPCpvCqvpPBQTlQLrdlJWrLJLmrlrr", "psSsBptpCBdjtcjNVcGG", "sssppsmchwspFLtvHhQJMtFb", "rRLDqRVLNSMvFSSV", "WDrLnzrDqzRqRzzfLgRnzrnsssZZZsZBgCwwmBppwBspZc", "MtPbwvzzVtzfsqGGVpdSjsLd", "DnNRCDJBnHJDHDnrDTRcnNZpZSsSLqjpdqqqSMqdddTj", "CFRFRFHgDRnRgMtfFvvwzwvwvzWv", "PNpFPncvvchPpNjpFhvPhPLmBwMgDRRwRgMDhBmLzBDD", "trHSrdTtslWrSWmfzgmMnfBzmn", "JsrQsJHHlsVqTjjpbbNnQNNpGG", "sVQCdsmGlnlCmnGmQQhGCJJNvNjpgqhqhvPgpgPqjpcpcW", "DMSfSbHLHbSDBBzLNvccWRcddvPjNj", "FSwHfTFdFsmJlnTTmV", "FctwtTTCScvShFqtwScrcTSCJQGNndGHWJNQHWHZdgJrJgGN", "jspLlfPlpfsDjBspfllWgdWHQggZHngHNGdsWJ", "lRMpBlPmSFRTcvZv", "TVZpRRVvFRVpTZRfFhFvvzGVwrwwwdDBMwQrgcDtMtDDwZrL", "jsNsWqWjNQCNWbjPMcBLPgBtrdMwdrdL", "SJSsmqlSNljbmlNjsbQVffzGRvzmmVhQpVGz", "BFFMvcwMwwpFFfpbDMqPVgLVgmLDPR", "JtSsSzJssQJJWjRZzgLnDgqLPZgzbg", "dsdtWQWdRGjTRNQNQvGfHCCFpvfGwlCfCp", "zzWGqWnqnwWCvCrHffHRpBpBBRSJzRFFDhSQFR", "VPVsZMNTLsMvPsmBJhllFpFBTJFQpl", "sZtsNsZtdZgjbwggjnbvqr", "QpTvrphmDvvddfcJJHTTncMlMG", "bRZZPRwjgzzlSSjGlnlSJS", "BZzBzssWgwzzwNBsgsPBgszmQptqrrGdvdWmqdhhFQDpQF", "FfMtzSqlDlzfMhPFhPtffNRsCgSgCCGspRpRGSsgsg", "WTcWLTTVnWmrVdLrcHmNGNCZwCCggpgDRwpCnR", "JcdWJHTJWdJdjJccrLvlFltFQQvPFDMjqqlP", "NfjFNNZPDQVJVWpCbQpJ", "lcmdzlmzBtRSTlTTcncsVSbhpLWpWgsWghgsVS", "mBTRRmGccRtBwDbrGPbrMMrF", "VVQqlsGrVsMWBNFNMQHF", "TfzZfDgjgnLGjjztTncCFwwNmdvNcwwNBvHNFwvd", "jzjDgZzjntGJZzCnhrrSlPVsJslpslPq", "dVhpjGPdjHhqHgtHJJ", "sFzrzllQswDwFbcmBlgvZTCgvqTCfgHQJtqT", "nDbBsFzzrrtFrlwzPdSVMWMVVMSnGNLd", "wqJCjqChmwMLmMmprNgG", "DNTtdsdWcHdNspGQggnrgLnQpc", "SZvFftdddDsDTtttTDJNCCwJJZzbbCjwwVPj", "VstwZCwslBZQDBjfDDBDfS", "rvHnmMRrTzmMrmhRppbhDfpjfbjbctjD", "rPFLRLmFvvLvHvTCtqsVVwldGZGPGV", "SNZDJGfvwgMgfgmLmLcmBqgWgQWr", "hPnPTnVGsPRqLWpTmQqQ", "tsbnFhhjhSdGJNCjSf", "QSbGgBjfTCMWFNPFFtDghPrP", "JqHqJVzZzwJJHLlqQptDhPrctpPDtnLF", "zZmwJwdwdvHddVJvZqdzHlfGBbsCmMTsWCBCjsGGBbMQ", "gVmLtpWrFTFBLtpcFNbvhNNnTnvnQRlQQv", "GHjjqsMqwZZJdqGfZjfZGjQDDhvQNzznzRzRwDNbvgzv", "sZGPgssMqHZpPptmCcpCrP", "CZCNNLmwzwCGMZQMQsFNWplvpsJBWpFHBp", "SPbSbDRRbnDqggnbVbVrbRWHJFTlBBcTvWvsPvFpHTcl", "tVgSvqVbnqDdgQzMQMGMmzthCw", "mpbPQlblbwSlfSPGBpBGPpBFgMMtLFSHMLVVDVdtHLNctF", "WnhZsrJhTnWrgZdZgHNLcLtt", "hjCsCWRhjzhTjnWnQBbmPQQPLBGfqjwQ", "BFrzdtmRmpFtFwwmjjzNQllPshqHvjNh", "fWCLLCMJnCDbgfMJhPsPHvqvqVsssQ", "WbLnCZnvprpmZrGd", "gjMzTGBjWFBCCSSrBC", "bddJJndbdJfwPPthrrSSSsFSSg", "NJHbZbbvvWGgjWgT", "dpfphMggHdQcwftMMgdtzWGfGWnDBnmvnVJVvfmn", "CqPFTZPSNCTsZZZRLzGmcGVzDLGvLWBWJm", "TCrTNSScPlplpHrQrQ", "lqrCvhWFvMGWgfHPgLfjfdgG", "zjmbjSnzRzVVRmzBRtwjVQnNLfdPgfLdfTtNLPHTNNpppf", "mbSZRzQRBnnzbQJbmjmSbmVhlslZMrclWFrrqWCWsFhchF", "mpfNshshflNthWfJCBBdmnQbQBZQdn", "VFVRccgGTqTrHTbWBFjJBCFFJCCQ", "PvTDHqHqPPGVqqhwfNlWlDhMltlh", "HmLLgWVjJwhwWLgjjhmVHLLLlSzBlBlSvBvBFGvtdStJSSvq", "TRRrPMsfQTbRRCZRnTMRZZTCcvSccqBqBBlzdFvBqFsGBcts", "bRNPbRCZMCrQNfPLdhVpLDVgDhNHWV", "sPJFDsSsVLgHjLHPbj", "vCnRQhhRQdVQZlZdbHNMlqNNjgjbpbBL", "ChhCCCWTWnnmvmGtztStzScwrcVcFW", "NRBTNDBglSSgDwCClQQSFFHdLLsFbPFFLt", "McpmWccMWHZPcLstbt", "MWnWphVMvvzJzpWJWmVphjrDDBBTRwRDDDRRCnPDnqgg", "hsnnhhLljLPTmZwvdZdZjmmz", "RQNNDpNMSZwvsmqstN", "QFMFRDVDsHSpRpHSMRHfGGGTLhCChBGhBhBFBJ", "TTbltCvClzvzCZtwtwLTtQQQgjNgmjgQRRRQSjQLjR", "HPpnZHcJsdnnfsdVHHSSqghhmqBSSqSmBfNj", "VMJJdnMPrMGVrzvCZwZWCzCt", "CcQnBBCfBvRzDlsS", "PdbPPCbhGGpDSDlDDhvDJl", "HmHWPWdbHnCwCHCr", "JHlmJcMWHQcPmlmJMmMZPfwTTRDfgdDBfRtgQgdfBw", "zVvFrqGjzWGVrqvjvNFpspvVBgNNddtTgghhRTwgdDwwwBfT", "bVFrrvsvjWGVsCsqpSHZmMPJlJnCcLcPZZ", "QrrQZFZnRtnFRTrnlFTtRZwpGGwzGszhjzbsGzzhmjjhhmhf", "SgvpDSJSgPBSDPDNgpggmsMhMmffsMmzfJmzHHMj", "WNVcCPgSSDCddSdWCpNBclCQQFRQQlnTLFLRZQnLtt", "tgvqWqzvntdlzNzzHrbPrMhHMhhrVrjPmC", "FGTGGRGTffcJRpQcJJwmjhPLhjnCQPrLPVQrMr", "SppRSDRwFnJJwwswTDdtzBzqWWzZDldNgtvl", "sjzPjLphMSrlppSp", "fbGqgwTqgVTFFcgGTTGcQvfHQZRlSrSlSmMNMBRNNNrZmm", "MVcFGTFtqjsJLjjtCd", "jVJqTRHjjtDjZnVJVVjnNdwfCFPZmPgCCgCZGCFFrrFg", "LSLBMBWsbBBCgfdrFd", "ShbbzdzQRNNNVhDN", "JwbSRqmSwRwcpmbSSVfhNBTVGGRnNNzRNh", "ZPfQPjZZrffQZszhzTnVGBvntsNz", "rLjggjQgQgFdPQjLFQZgwLMbWccLSwWHqHfSqpwc", "cBhzNpztzHNrpHSHQrhZjZqdJRRgPqjhjqqP", "bQwsMTswwVjdZMVRJM", "vGWCTDCTQvvtlvQQ", "mHVmTTsmzRmRHffmmfMGBzSPMhSWWdDWDSGB", "VNcnNbtqqJbcbvgvWhShDBDPhjgPhSjB", "FrrbCnqJcbCJcFFbnqJlppCTfQmfmZQTLVswRLZp", "rWWppSStfRBLfHPdHHDzlldZ", "QrTnQmmVCqvrvJvzJZPHvzHsDs", "qgMMmrTTCcnQQmCNQNQmgTpwRWRtwtpcBWwjSRSSpttW", "SzWQwwqVbQzwGSfVwffVwbqhFGFFMgGLGMlNMcnNgcvlMg", "ddZHHsrCdBssDsHmChFLNNnMvmNnnnFncL", "rpJJdHZJdZHQSRJbSfVhww", "jLtFhNwNNvHnjFVvQLQLHhfbGDMPfffpllpttZMPPZZb", "VmrqszVmgCWWggbPGclPlZbcZCbf", "JdrTmVTVsqsRJHvQnnhwhBLH", "vpdBvsvdVvSPhvQFCgrRmhjqqRgWcWWgmf", "tJDGbZHbjRHNrrBj", "DnMnzGJJwtzVSzzBzTBSTF", "hZMDbQhMhlMDJrDrPMJRRqGJSvFvSwFSVdvGvwSFqq", "HjjzcRTnpGVSGGFWHH", "pssmjctLzpsRRnhlQbPfPrgDbZls", "FJMhPhnTMGmBFdnmlrfqfCcwlwCwCv", "pjHLsLbjjRHsHsjDjVsgDRjzCnfCfzqrqvwgflcvcvvgqz", "njtpHZnLLQGhTtTWSSSh", "NMddMTDrrjNnrnMWMrlnPwNwftBtGvptGjBHgBgpfCCSfgvf", "mqRqLFZRVFbtgBSCbGWWHp", "LZZRmQhVLLFzZdTQPndPNTTnQW", "CprQrcpqhHhpppchpphdcdpLMRMGsVGGsMsLbZbtbfMRmtbG", "DzlgTjwPTlSSSCDSlggNmMRtbsstsMfbwtVssMmt", "TNCNSBNJJTNPJjCTBDzjlJrvvcQrrhphWBqHhHdqrnrr", "ScbcbglMPRSmvclTlzMTdhVHhhjfdsdhrVDzfjrD", "LJGtpQwpQBBCttJLGqtqGHhPfDdHwjjhrVrhdrdshr", "WCpZQCGqtqQWtQWplZPcvMmgmvPccbvT", "tGWqthqGVdzBWwdd", "BZvZHFFHZrHZvZNRRHHZNHrMMTzRMTdTzbgDVMVssTDTwg", "JnNmrHHNrCGBPPnGGc", "wvFvZBmppBdSLcvshcLrgl", "bTQqJJHJzJjDQjQtzRVJztDHslgNNVLcgSrsSsWrSslWgrgL", "TqHHjQnbHTbbmZBBcFMpmBwn", "ZFCqSlCPdCRTLWWTQQLLQS", "GjggHcvsggHVTVBLnlcpBQ", "hvGhlrtjmjrHHhjsMPFNPdtqMPqqRfCf", "cgRwVfVzrgqqwZctTztFMFMTCdPmmF", "bWJsSJHpTDRDTWMC", "sHHNbshnlJhJjBsjsbnBHbZcqrZZQwqvvZcrwwvRcZnr", "ttgVBtMbttngmHJVpzlzZlppzw", "cPcsRPmPPQfGcccRQSSZzzvzlflzZHvJppzJzT", "sQPSCPPWccWWWqsQqRCQFNRMmnbnjjNbgDdBjhBrdbdDbd", "gSsZDSgdQZgWSgddwLDLLfCDBqvlhBlClqtqjhvBhB", "RMcsVMcsbVsrPrmJcPHHVvfjjCnvvfCqhlqvqqqq", "NFMRRpHPgzszGppG", "WmmZZNJCgCBZCzPvmBCmjWjjqFtFqjFjbVwqwjtt", "ppDMpdMfSfSSNtlwqbqwwq", "dTGDhdnfLMLhfDDpQvZNZgmrmrQJgrLQ", "jrqRqfNNhrVvcTVdpTscpd", "tGWBlLggWGddTspZZd", "JJzWtQgQsDbBgDWsgbFrPrRJNMJMPqMNfHjH", "GdvVbbWsWdvWGDvWZcbFBTBZCzCjwzrhPBJjzF", "nQPfSHnglfpnMSngnMgJTprJCCwwrwCwpTpCFj", "SQQHSttRqggmlVsNcvWdRvbWPN", "WLNLCWLsSJgHFrSHrM", "gDbzRTcmbzPPgPPdtBctfHfrMMQfGFJrhQnMfGJR", "dmmzzqZdDqqTLLwjgZgNljws", "TCqsDHssmDsDGVGlVcMccGDV", "FLntWWnnFzFgzzbtbznLBtlfjGZmcfFGVNFGlVZcMNlN", "PpbLLmpgbRzqpCHwChQqwh", "lVBPVgflgBVVrVTTwgwBPmgflDQjnnGvqjGDDFjGlGnvqHHJ", "ScCLzdCZdbLSbFdFqvDDdGMQGQ", "bchNCLRWSbzCbNRzbWRmsrhQwVVwBhmQpPwpgV", "GmfHCCPqWqHLWLCfRJpbrrbQphwZbZ", "STVDQzNnzMzNdstMDzzNtZwFhrlwVJJrJlFFJRFwFR", "DzSvzstnszMnjQcqggjPqQBcqq", "BzNwLTqwTjrBrljWpBlrQpBpsCGGCtpssCMttDtHsttMDCHp", "mnrJVmRhhvJVcHtsGHMCnZCPZM", "bvSmVbRhbgmmSJflzNLrzqzfWfqBWr", "qsZLqqFNhfrGnJFv", "DVjVmWlcjVjdDTDlbbTDTzmrCnMmJGnHnmCHfJGfvnfM", "lVRdDglTlTgVlVjbWWVzVjvNZqSBsLZNtqRsLBqLqQtL", "ffRRfLVHZHfJHVJcMrMZZwwTzGBBzBTBjGTwrwdSCj", "qmWbWQbqnbvWhbQPGnGNnpvwSzltpBldtSdzllptCSjTTt", "vWsbnbWnNvQQQQvsRRLfJRRVGfDcfV", "PpqrvswPvvvPsNqmDLDttCCcHHZVrCtW", "hgSddBhghQMdbhbwhgSdQgCCctZcDtLntLttCVLtSLCc", "dGzdQMFzQMdgGGlGJMzBgBwRqNNfmmTsvfRfJTpwjmpP", "pcphpdrWDmTgHWMtRWqHRVVH", "SbGcGGQnvNQGvsjBtVHBLjvqVPVB", "lzbQCzbZllSzQslpCJpJJDggcggfDT", "ffrTlhrVrfCsDVTsWzGBBZcBRBqRBWtGtZcq", "QNFmFHrHNSSmSLJdJrNGZLbGjbcBjBGjqcbgjc", "ddQvPnHddnQJHSHSFPdnFfDDTfDMMCrwMlPTwDCsrC", "NBnrNHQHBscvhfBM", "bbGtgWWWqZFmnZGZbmVCdMzvvShfzfGcdcfMSS", "WqbjnmbnjWgmFFgtWVbWZwNTNLjNprNwQLDwrTrHQr", "lwwlcjlzQRBcBccbdLCVnNVWJJzfLndd", "pZDMtZpSDMZpMZMsPtHVNJVfRCRddVndnJLSJd", "mDTZvTqtDDHttRPcqhQlwgGlghbqBh", "DTspTqssqTbdmCMwVmnmlfCD", "PLPWjjrSjvPzqZvjvRqjLtPwnnHCmWVcncMnnwmncHcHll", "RFztFjjqjQFQTbdTbQ", "hTFSVSdhMMVMFjjgbthcbzczcg", "DvwCJVJWWJDRnfmbDmccfmmgfb", "QrJvPGWWnpVMqdZsZpqM", "gRLcHbgnfpgpJjlqqp", "SFwrTHFBqlZtBPZq", "HTzDwmSvCvCmsmmvTSwvFwcfRRNRLcQRWNssNNbRWLWL", "PpMgDMDnsWSnjBQnrjbn", "LFcVVGChCFdhdVFZVpVCdVbvQbRrbvBBbBjQSJZrrJrR", "FNwGNCCFHcVTHcFdHHHTDzMzfsgzwpPWMmPflmtt"]

console.log('part 1 sample', part1(sample)); // 157
console.log('part 1 input', part1(input)); // 8176
console.log('part 2 sample', part2(sample)); // 70
console.log('part 2 input', part2(input)); // 2689
```
