# Day 6: Tuning Trouble

Codepen: https://codepen.io/mamunoz-dev/pen/oNMeVVP?editors=0012

```js
function getNumberOfCharsBeforeMarker(input, startOfMessageMarkerDistinctCharacters = 4) {
  const codeArray = input.split('');
  let lastMarker = 0;

  codeArray.reduce((acc, currentChar, index) => {
    if (acc.length === startOfMessageMarkerDistinctCharacters) return acc;

    if (acc.some(char => char === currentChar)) {
      const index = acc.findIndex(char => char===currentChar);
      acc = acc.slice(index+1)
      acc.push(currentChar);
    } else {
      acc.push(currentChar);

      if(acc.length === startOfMessageMarkerDistinctCharacters) {
        lastMarker = index + 1;
      }
    }

    return acc;
  }, []);

  return lastMarker;
}

const sample1 = 'mjqjpqmgbljsphdztnvjfqwrcgsmlb';
const sample2 = 'bvwbjplbgvbhsrlpgdmjqwftvncz';
const sample3 = 'nppdvjthqldpwncqszvftbrmjlhg';
const sample4 = 'nznrnfrfntjfmvfwmzdfjlvtqnbhcprsg';
const sample5 = 'zcfzfwzzqfrljwzlrfnpqdbhtmscgvjw';
const input = 'mvwvrwvwbblffmvmhmllmzmqzzbhbnnvrnvnmvvqrqrccpsstvvshvvpvvpddzwwjddfzzgbbzcchrrtzzmfffzqqsjjbfbjfjwffhrhrgrfrsfrsffbwwphwhgwgmgnggzwzhwzzpggnccdbcbssgccqgcgcvgccwffrqfqtqmqppwphhlbblnnwhwfhwhmhzhmmzbmbbjjmcjcggwlgltldttpccjlccbjccvfcvvdbvdvndvvmbmcbmmhphpbhhgjgllmljlqlwlclzlccrvrdvdzvzzzbrbrnbblplrplpsstdtjddgjgjgwjwzwbzwwrhwwbzwbwpwssvgvnnwnvvjqqswwsccwctwtntdndffjzfzmfzmfzmfzzhjzhzssdsgsnggbbqdqlqbllmglgddqppjjtzttlflrrczcmzzsppvfpvffcfrccvvvhjvhvfhfbbwdwrwswqqftfbfcfdfnfpnfflbbcmcqqsqwqqccmwmvwvffvdvlvtvqttnllqslsnllzplldmmzczhzmzzhllpfpqpnnqwwwrzzdpzzhccfvcvclcnnrjrmjrjlrlflqqtpqqdgqghhwdwdttlztttwmmvfmftfptphhddswdsdpdvpvpcpjjnvjvqvhvqqqfgqqgwqgwghwwmmvmvfmvvnvsslcclzlslppztzfzbzllcgllcsllvgvqvtqtsstztwwgjgghnhnhdnnqgnntqqtccdmcmhmpphpmmmbpbptpnnnmnppwlpwwfpwfwmfwfssbspslsflljpppbsspwptpwttfbtfftltvlttdmttllmggfcgfccvwcvczvvgvsgswsrrwppgcpggpgnngfnndrrdjrdrfdfmmjmsmbmmwjjjfzfdfbdbbtgbbcsbccgbbrvvrqrwqwmmhrhhbcbdbvdvzdvdjvvnwntwwdfwwrhrmhhnghhncccfttmpmrrjhhqddrsrvvssmrrgvgdgmmfqqhttspswpwlplpjljcjmjmvmdvvhbvvtqvqccczwwtlllztlzzpnnqznzddqsqvsshrrmccnppgrrjcjpccrvcvnvcncllffvnnbsbhhrmhmzzzhrhchjccgjcjwwhddmfmhhmrrrbccqddmjdmjdjtdjjmttzstsctssvvgghgbbhmhzmmjqqqqjgjfjpffpmpzmzszfzgzcggpmpnnqjqffgpghhszzvlzzdgzggqmggqlllcwwtzwwcfwccpjpggtqgtttzffdpffjzfzddzppprnprpqqbppvjpjzppbzpbzppwzzcnndsdzszbssqwqggnhggnzggpcpnndssghsggmfmrffvddvwvssjmjrmrmnndwdwhdhwwfjfjwjmwwqssmdsmdmssqzzrgzzjttnqnfqfcccvvmsvsvffccdjdccgtgqqjwqqnbqqnhqhrqrmrgmrrzjzrznzllwrlrbbcppctptwtfttlntnltnlnzzhrzhrzrtzzvsvfvjffsbsnbbfpphtptjjmffqgqnggnqnsshffslflggtlggmpggnfgfbgbfbcbbtdbdjqwfggzsfmzflttgdfqchhtjfwlmdnsbvmqcrhsrtwtjlmnlwbvjvqdzswdthbjslqzgmtzfjfcrgbhrrjtzgljbqfrzzqszhddnmzpnbgnflghnflwdmjdhgpnvwvnltcngwggqdznrpdtwsrclwwlfzhnjtpqcgzjqjnhcwbhndwvgrczhzwfjrdvjztdbjshmhrrqctjwpcdnpnvrtggrmhzhdtlntjphcddplgfvvmjzcbbpjbqbjwmnwqgftbmchghwvrlptvnfbffvgtdbtnfzwdmdlgzjbrvffvbrfwgjzzpbpcdzhcjbhfwnmqqwvjvnpgdqdjsmwdmrgfqrwhhqqjpfmvlncfdjchrccwpbptccdchqwvbcqzlhbfgrdzgncbwrzqpfszrcwtmnvbztjlzjlrqqfrnplhnmdjljcpnqssthhmjqrrlwdvsjswdwtfstmbvwhbptjnphjmnllbwffppzmdpchbcwcmgqzfmdqvhcmrvtgtjwlzfpnjnjtfvdhtjlqsdjwrrnflnsqrtfsnbjfdllhqslltsjqzdfqthqjjgrtqjwmlqqlhvszqswmdcbnwqgshpvfjdtvsqjvcnrnvtpfsgqszhtmcdlsqjwmttcqsdlvssrgwhtmtqwmttptnmgzpwzzrbwtsrjhmjpblztgftppcwwrjppvwlvdhwwdlfcdvhwpvhqpwrqsczvlmtghrrvqbphljcmcrfmwltlmnzchzrlbgspdjwfbhrmnfhvjwlgtghcpbfgdvrmwbqbprfvwpzqzrgqbhjtztwhjcjtncmbgjphsgdfvjwjljlwzhsdldqtdvgtzpwtmrbnpvqrmfdwngzqtsdjjztslrwdnfwgbnsjblcjzvmgbqllmdvvvdcvplgbzmhcrpbbjbfzhjgfpmjtrpmgvshzvqhcbcjzfcsvqggjcllcnjlrwqfpzldswzjgzvqwvszhrntnvlcpdcfqhqrtmhbdjqpfblrsbrhdfdwfgbhsnnjpgjvfpssfmhdbdncfrqbzpbttrhfzqnrttltqbvmhglbdpwmdbmgwcdsdsflmcnphwvbbhgqpqmwbdsmqwhcdftdlcfnstlfnzzsjhqzlsdmhpvlcqvhhlpdtzlqzlvbbwhhdsqhntbtpjjvnjlrncfmvnqmzwpldgrbfcfgdtlmrfzcbqfhvhpmsrrlnqwfggwrjsmnpnqhvvcfsfspfrmhwmbpqfhprzsswrczttlczhcqvgqqsnbhhfszqbswgtcjgddhngtcvlwqqmqzntrcwgwjhmhlgclpgtmqnpgwwhnjfdvvjgmqjlzsmwztpsdzrjlshswzljlmdzfqmqbtgtzlsfwqvwrdchjvrdnwdbprfvdvczstvnzfzzfmzbwjhtflbmhlgmfzrdfrqwfbltwqlqrghlprppvlqwggvczdcnmrmblhcfmpzdmqppgwhbbrjlzsvmsfzlrdljftcrdfdgmvrccwszpjmvdnbmfdnsgmgzsdpbrlsqvsmcwwqlwtwbgfvwtsmpqlnhjchnzrpncgtcqgwqrmqmrwsbmdvqcpggpzcjgtrhvnbpprlwfnjlvrgrdjjvncjmmflrpczqnlbqczggssqfcjcrghqlsztpdjpbbfcdjzzdjbljlsczdmrgshdscvhnltrwchjcmjzmjbhldnqzwwpswsnsldcbdcdpdgpjgwrnfbcpjtvzlhvgggldcfqcwwppvltsvsmwzwdgmhnfggtgtwldmrglcvmstgmbgbjhwwhgdhfrbjlwfhjfppdmffblbbzrplhlhqlsrnsthvjtglqntzcncmvhqnlsvvrscrhlncgncjswfgcvgjlwsndzmsbhbdqsggdgrsfqtzwnjpsdlbsqjrrjwwlbptwqpfqwvpsrtrmstddzdbjjlwvfqcpfczpsnmcgbfpbwcpljdhrgsqftbnplccjphwsdbprqfqqfcvtcdznnhrbdqpccphvdgtspmzzdbnslnrvtfrtbhcfzfgmhrttmdpwftvccjbllhqgtmpgwvbdjgtvtfbfnnsfgzqjmrpbcmqhpfbstznbvgtbhnwbbnjfsthdgdrpfdtvrtmgbwzqqpnlltbshjvnhsmqhqwzgbsfqqccfznjtnnzdrgcvwnlffdgvvqzvwhwfswdmqlrsglntzsnwjzgrhhwzzshwsfmlmrbnmrqlptmjgmtqctrmddzghsgtrbbcsmhtcnrzwmvrjmrnmhbjmflrclvlbzwbgmtnmwqgfmbbnnrdvhqcflglvzbmjzjtvnmrbgghnccfrphjshsgtrhfmmghhpwgclfvzfbdccsfrlfwtsjjnhlndpwcjdtlllhcsvwrwsbppqwhfcvnsnrvthrsbgmgjhpmjdndmdqdgzfvbqfmgfjrnrjchstrjprfwfnjqblhjdgsstvtpcsvmpbhggnwzncpjdhrcllcghhprhwhfgsqpfzphrdlcbrccglsb';

console.log('part 1 sample1', getNumberOfCharsBeforeMarker(sample1)); // 7
console.log('part 1 sample2', getNumberOfCharsBeforeMarker(sample2)); // 5
console.log('part 1 sample3', getNumberOfCharsBeforeMarker(sample3)); // 6
console.log('part 1 sample4', getNumberOfCharsBeforeMarker(sample4)); // 10
console.log('part 1 sample5', getNumberOfCharsBeforeMarker(sample5)); // 11
console.log('part 1 input', getNumberOfCharsBeforeMarker(input)); // 1578

console.log('part 2 sample1', getNumberOfCharsBeforeMarker(sample1, 14)); // 19
console.log('part 2 sample2', getNumberOfCharsBeforeMarker(sample2, 14)); // 23
console.log('part 2 sample3', getNumberOfCharsBeforeMarker(sample3, 14)); // 23
console.log('part 2 sample4', getNumberOfCharsBeforeMarker(sample4, 14)); // 29
console.log('part 2 sample5', getNumberOfCharsBeforeMarker(sample5, 14)); // 26
console.log('part 2 input', getNumberOfCharsBeforeMarker(input, 14)); // 2178
```
